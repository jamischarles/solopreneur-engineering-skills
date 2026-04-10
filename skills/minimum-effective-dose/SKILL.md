---
name: minimum-effective-dose
description: Build the smallest version of a solution that actually solves the problem. Use when the user mentions "how should I build," "what's the best way," "I need a system for," "the right way to do X," or when the user is picking between implementation approaches. Distinct from scope-discipline (which is about whether to build at all) — this skill is about HOW MUCH to build once you've decided to build something. For deciding whether to build, see scope-discipline. For task planning, see task-shaping.
---

You are a solo developer. Every feature you build has two sizes: the version that solves the problem, and the version that solves the problem "properly." The properly-sized version takes 5-10x longer and, for a solo project, usually provides zero additional value.

This skill is about finding the minimum effective dose — the smallest implementation that delivers the actual outcome the user needs. Anything above that threshold is waste.

## The principle

**Dose-response applies to engineering.** In medicine, a higher dose doesn't always produce a better result — past a threshold, you get diminishing returns, and beyond that you get harm. Same with code. A "more complete" solution past the effective threshold adds maintenance burden, surface area for bugs, and time-to-ship — without improving the user's outcome.

Your job is to find the threshold, stop there, and resist the instinct to keep going.

## The core question

**"What's the dumbest version of this that would still work?"**

Not "the simplest" — "the dumbest." Simple implies elegance; dumb implies embarrassment. If you're not slightly embarrassed by v1, you went too far.

## The dose escalation ladder

For any feature, the right answer is usually higher up this ladder than you think:

1. **A static page** with hardcoded content
2. **A manual script** you run yourself on demand
3. **A scheduled script** you run yourself once a day or week
4. **An automated job** that runs on a schedule
5. **A real-time system** that responds to events
6. **A multi-tenant, configurable, self-service system**

For each proposed feature, start at step 1 and climb up only as far as you need to. Most solo-project features stop at step 2 or 3 and never need to go higher.

### Examples

| Feature | Maximum dose | Minimum effective dose |
|---------|--------------|------------------------|
| "Send users a welcome email" | Email workflow engine with templates and segments | Hardcoded HTML string in a single function |
| "Track which features users use" | Full analytics pipeline | Log lines you `grep` once a week |
| "Let users export their data" | Configurable export system with format options | A "Download JSON" button that returns everything |
| "Generate a report" | Dashboard with charts and filters | A script that prints a table to stdout |
| "Let users invite teammates" | Team management system with roles | Manually add emails to a database table when they email you |
| "Rate limit the API" | Token bucket with Redis | `if (count > 100) return 429` in one file |
| "User settings" | Settings page with categories | One dropdown in the header |

In every row, the right column solves the problem. The left column is what solo devs build when they forget to ask the core question.

## The two failure modes

### Failure mode 1: Building the cathedral

You're adding one feature. You start building infrastructure for all future features. Two weeks later the original feature is not shipped but you have a "flexible system" for "future flexibility."

**Symptoms:**
- You're writing base classes before writing the concrete thing
- You're building a config format before there's any config to store
- You're designing a schema that "could support" things the user hasn't asked for
- You're solving a scaling problem you don't have yet

**Fix:** Delete the infrastructure. Write the concrete thing. If you need infrastructure later, extract it from the concrete thing.

### Failure mode 2: Edge case paralysis

You're writing a function. You start thinking about every edge case. You add handling for each. The function balloons. You spend most of your time on cases that happen 0.1% of the time.

**Symptoms:**
- Most of your code is in `if/else` branches you've never tested manually
- You can't explain what the happy path looks like in one sentence
- Your function has more defensive code than logic code
- You're handling errors that your internal code can't produce

**Fix:** Write the happy path. Make it work for the 95% case. Let the 5% cases throw errors. You can add handling when an error actually happens. Most never will.

## The "would a dumber version work?" test

Before you start implementing, go through this checklist:

1. **Would a spreadsheet work?** For any feature that's "storing and displaying data," the answer is often yes. Google Sheets + a public link is sometimes the entire MVP.
2. **Would running a script manually work?** Automation is only worth it past a threshold. See incremental-investment.
3. **Would hardcoding work?** The solo-dev cheat code: you're the only one who can change the config, so hardcoding IS the config.
4. **Would a third-party service work?** Stripe Checkout instead of a checkout flow. Resend instead of an email server. Supabase Auth instead of auth. Use the hosted version.
5. **Would an email work?** Notifications, alerts, reports, updates — a lot of "features" are just emails in disguise.
6. **Would a `console.log` work?** For observability and debugging, sometimes the MED is literally `console.log`. You'll graduate to a logger when logs become load-bearing.

If the answer to any of these is yes, stop. You've found the minimum effective dose.

## Resistance you'll feel

Applying MED will generate these objections from yourself. They are almost always wrong:

- **"But users will expect more."** — No, they won't. They'll expect the thing to work. Most users never notice what's NOT there.
- **"But what if it scales?"** — Deal with scale when you have scale. Most solo projects never reach the scale that would break the minimum version.
- **"But this isn't the right way to do it."** — There is no "right way" that applies outside of context. The right way for a solo project is the way that lets you ship and learn.
- **"But I'll have to redo it later."** — Probably not. And if you do, redoing a small simple thing is faster than building the complex version upfront.
- **"But it'll look unprofessional."** — It's a solo project. Polish comes after proof. Ship the unpolished version, see if anyone cares, then polish.

## When MED doesn't apply

MED has limits. Don't minimum-effective-dose these:

- **Security boundaries.** Auth, payment processing, PII handling — use battle-tested libraries and patterns, not a "minimal" version.
- **Data integrity.** Schema design, migrations, backups. Cutting corners here creates data loss.
- **Things you'll touch daily.** Core tooling you depend on to work deserves proper investment. See incremental-investment.
- **Correctness-critical code.** If a wrong answer would cause real harm, do it properly the first time.

Everything else — features, UI, internal tools, admin flows, reporting, notifications, integrations — lives at the minimum effective dose.

## Verification

Before you start building:

1. Write the dumbest version you can think of. One sentence description.
2. Ask yourself: "If I built only this, would the user's problem be solved?"
3. If yes, build that. Stop there.
4. If no, what's the smallest addition that gets you there? Add only that.

After you finish building:

1. Can you describe what you built in two sentences?
2. Is there anything you added "just in case"? Delete it.
3. Is there any configuration? Is a real user asking for that configuration? If no, hardcode it.
4. Did you ship something a user could use today? If no, you went past the dose.
