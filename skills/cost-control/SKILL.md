---
name: cost-control
description: Stop hammering paid APIs and third-party services during development and production. Use when the user mentions "API costs," "rate limits," "caching," "mocking," "Tinybird," "PostHog," "OpenAI," "Anthropic," "credits," "free tier," or when adding any integration with a paid third-party service. Covers mock layers, persistent caching, refresh-on-demand UX, and instincts for solo budgets. For shipping discipline, see ship-daily.
---

You are a solo developer working on a side project or bootstrapped SaaS. You do not have a company credit card with no limit. A single dev loop that hits a paid API in a hot path can generate a $400 bill overnight. Your job with this skill is to keep that from happening — at every layer: dev loop, CI, production.

## The four failure modes

Every cost blowout maps to one of these:

1. **Unmocked dev loop** — You hit save, your dev server reloads, your dashboard re-fetches, and every reload runs the same paid queries. Multiply by 50 reloads an hour.
2. **Uncached production** — A dashboard page loads and fires N fresh queries. Every visitor re-runs them. You just turned a visitor into a metered event.
3. **Runaway loop or bug** — A retry loop without a circuit breaker, a webhook that calls itself, a recursive LLM call. 10,000 requests in an hour.
4. **Leaked credentials** — Someone else's bot is hammering your key. Not a bug you caused, but one you can still prevent.

This skill targets the first two. For the third, write tests. For the fourth, rotate keys and set per-key rate limits in the provider dashboard.

## Layer 1: Mock layer for local dev

**Rule: you should be able to work on the UI without ever hitting the real paid service.**

Add a single environment variable like `MOCK_<SERVICE>=true`. Gate all calls to the paid service on it. When set, return realistic fixture data. When unset, hit the real API.

### Pattern

Create one module that wraps every call to the paid service. Every other file in the codebase goes through it. Do not allow direct `fetch()` calls to the paid service elsewhere — they will be the one you forgot to mock.

```ts
// lib/<service>.ts
const IS_MOCKED = process.env.MOCK_SERVICE === "true";

export async function queryService(params) {
  if (IS_MOCKED) return mockFixture(params);
  // real implementation
}
```

### Mock fixtures must be realistic

Fixtures that return `[]` teach the UI nothing. Bad fixtures hide bugs. Good fixtures:

- Match the real response shape exactly (same keys, same types, same nesting)
- Contain enough variety to render the UI meaningfully (2-5 days of data, not 1)
- Vary between calls if the UI expects change over time
- Can be parameterized by query/input so different callers get different fixtures

### Signal the mock in the UI

If the dashboard is showing mock data, the user (you) must see it. A yellow "MOCK DATA" pill in the header is enough. Nothing worse than debugging production numbers that are actually fixtures.

## Layer 2: Persistent cache for production

**Rule: a page load should not trigger a paid query unless the user explicitly asked for fresh data.**

The PostHog model: dashboards load from cache every time. A manual "Refresh" button is the only thing that invalidates the cache. Users see "Last updated 5 minutes ago" so they know the staleness. This model is correct for analytics dashboards, reporting tools, and anything where the underlying data changes slowly or the user doesn't need second-by-second accuracy.

### Cache storage choice

| Use case | Storage |
|----------|---------|
| Serverless, cross-region, need persistence | Supabase / Postgres table |
| Ephemeral, single-region, high throughput | Redis (Upstash) |
| Per-request dedupe only | In-memory `Map` |
| Static asset | CDN / `Cache-Control` headers |

**In-memory `Map` caches die on every cold start.** On serverless (Vercel, Cloudflare Workers, AWS Lambda), you will get a cold start every few minutes under normal traffic. An in-memory cache is useless as a cost control measure there — use a persistent store.

### Cache table shape

Minimum viable:

```sql
create table query_cache (
  tenant_id text not null,
  cache_key text not null,
  data jsonb not null,
  updated_at timestamptz not null default now(),
  primary key (tenant_id, cache_key)
);
```

`cache_key` encodes everything that would change the result: query name, date range, filters. Upsert on write. Read by `(tenant_id, cache_key)`. No TTL — invalidate on explicit refresh.

### Request flow

```
1. User loads dashboard
2. Server reads cache row. If present → return it with updated_at
3. If absent → call paid API once, upsert cache, return
4. User clicks "Refresh" → server passes ?refresh=true
5. Server always calls paid API, upserts cache
```

### Response envelope

Every cached response should carry its own staleness:

```json
{
  "data": [...],
  "cached": true,
  "cached_at": "2026-04-09T14:32:11.000Z",
  "mocked": false
}
```

The client renders "Last updated X ago" from `cached_at`. No guessing.

## Layer 3: Instincts — before you write the code

Ask yourself these questions before adding any paid API call:

1. **Does this need to be real-time?** If the user can tolerate 5-minute staleness, cache it. If they can tolerate 1-hour staleness, cache it harder.
2. **Is this call in a loop?** Loops kill. If the answer is yes, either batch the calls or precompute.
3. **Does every page load need this?** Most don't. Move it behind a click.
4. **What happens if this fails?** If the answer is "the page breaks," you need a fallback. If the answer is "we retry forever," you have a cost bomb.
5. **Can I mock this for dev?** If the answer is no, you need to restructure until yes.

## Provider-specific guardrails

Regardless of caching, set these in the provider dashboard:

- **Hard spend limit** — most providers support this. Set it below the threshold that would hurt.
- **Rate limit per key** — even if you're the only user, cap it at something sane (e.g. 1000 req/min)
- **Separate dev and prod keys** — when you blow up the dev loop, it hits the dev limit, not prod's
- **Billing alerts** — 50%, 80%, 100% of budget

## When NOT to cache

Caching has its own failure modes. Do not cache:

- **Authentication tokens** — caching a stale token is a bug that looks like auth working
- **User-specific writes** — create/update/delete operations should be idempotent at the API layer, not cached
- **Real-time data** — stock prices, live chat messages, current user presence
- **Data that drives user decisions right now** — a "Buy now" button should hit the real price endpoint

Analytics dashboards, reporting pages, content lists, search results, and leaderboards should almost always be cached.

## Red flags in an existing codebase

When you audit a codebase for cost control issues, look for:

- `fetch("https://api.<paid-service>.com/...")` scattered across files instead of a single client module
- No environment variable toggle for mocking
- `new Map()` cache variables at module level on serverless
- `useEffect(() => fetch(...), [])` in React components that re-run on every mount
- Cron jobs that fire paid queries without checking if the data actually changed
- Webhooks that call paid APIs without idempotency keys

Each of these is a cost bomb waiting to go off.

## Verification

After adding cost control to a service:

1. Set the mock env var. Run the app. Confirm zero paid API calls (check the provider dashboard or network tab).
2. Unset the mock. Hit the endpoint once. Confirm one paid call. Hit it again. Confirm zero additional calls (cache hit).
3. Click refresh. Confirm exactly one paid call.
4. Kill the dev server and restart. Hit the endpoint. Confirm cache survives restart (this catches the in-memory vs. persistent cache mistake).
5. Check the provider dashboard after a full day of normal use. If the request count is more than ~10x the number of refresh clicks, something is wrong.
