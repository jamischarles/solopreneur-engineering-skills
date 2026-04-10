---
name: ship-daily
description: Ship something every day, even when it's small. Use when the user mentions "I haven't shipped in a while," "long branch," "merge conflicts," "deploy anxiety," "perfectionism," "stuck on a feature," "can't finish," or when the user has a branch older than a few days. Covers small PRs, continuous deployment, deploy-first mindset, and breaking work into shippable pieces. For scope decisions, see scope-discipline.
---

You are a solo developer. You have no team to unblock you, no PM chasing deadlines, and no one who will notice if you don't ship for a week. Nobody will notice except your project, which will quietly die.

This skill exists because solo devs stop shipping in slow motion. It doesn't look like a disaster — it looks like one "almost done" branch that keeps growing. Then another. Then it's been three weeks since anything hit production and the branch has merge conflicts with itself.

## The first rule

**Ship every day you work on the project.** Even if it's a typo fix. Even if it's one line of CSS. Even if it's a README update.

The daily deploy is not about productivity metrics. It is about keeping deployment alive as a muscle. The moment deployment becomes scary, the moment `main` diverges from production, you have stopped being a shipping project.

## Branch lifetime ceilings

| Branch type | Max age before merge |
|-------------|---------------------|
| Bug fix | Same day |
| Small feature | 2 days |
| Refactor | 3 days |
| "Big" feature | Doesn't exist — break it up |

If a branch is older than this, something is wrong. Either the work is too big (cut scope) or you're avoiding something (figure out what). Old branches rot — the longer they sit, the harder they get to merge.

## Breaking "big" features into shippable pieces

The trick is to separate **user-visible behavior change** from **code that enables the change**. You can ship the enabling code first, behind a flag or just dormant, and then flip on the behavior later.

### Patterns that work

- **Dark launches** — deploy the new code path but don't route any users to it. Monitor it silently. Flip when you're confident.
- **Feature flags** — even a hardcoded `if (false)` at the top of a handler is a valid feature flag for a solo project. No need for LaunchDarkly.
- **Database migrations first** — add the new column/table in PR 1, populate it in PR 2, switch reads in PR 3, drop the old column in PR 4. Each PR ships.
- **Scaffolding PR** — create the new file, add the route, have it return `501 Not Implemented`. Ship. Now the infrastructure is in place and the actual logic can be a separate, smaller PR.
- **Refactor before feature** — if a feature requires touching messy code, refactor in one PR, then add the feature in another. Two ships instead of one stuck branch.

### Anti-patterns

- **The "I'll split it up later" PR** — you won't. Split before you start.
- **Rewriting adjacent code as you go** — every drive-by fix makes the PR harder to review (by you, tomorrow) and harder to revert. Stay in scope.
- **"Let me just add one more thing"** — no. Ship what you have. Add the one more thing in the next PR.

## Deploy-first mindset

Before you build a feature, make sure you can deploy it. Before you build the whole thing, deploy a skeleton.

### Day 1 of any new project

1. Create the repo
2. Create an empty `index` page that says "Hello"
3. Deploy it to production (Vercel, Netlify, Fly, whatever)
4. Confirm you can access the URL

**Now you can build.** Every subsequent change ships to that URL the same day. You never have to ask "is this going to deploy?" because it already does.

### Day 1 of any new feature

1. Create the new route/page/endpoint
2. Have it return a placeholder ("Coming soon" / empty array / scaffolded JSON)
3. Deploy
4. Now build the real logic

This eliminates the "it works locally but not in prod" debug cycle from coming at the END of the feature. You catch it upfront when you have nothing to lose.

## Continuous deployment setup

**Every push to `main` deploys to production. Every PR gets a preview URL.**

If your project doesn't do this, stop and set it up before you write more code. Vercel and Netlify give this to you free with zero config. GitHub Actions can do it for anything else. The friction of manual deploys will kill your shipping habit.

## When you feel stuck on a branch

A branch that's been sitting for more than 2 days is trying to tell you something. Ask:

1. **Is this actually done but I'm polishing?** Ship it. Polish in the next PR.
2. **Am I waiting on a decision?** Make the decision. Solo devs don't have the luxury of committees.
3. **Am I avoiding a scary part?** Identify the scary part, make it a separate task, ship the rest.
4. **Is the work actually too big?** Split it. See "Breaking big features" above.
5. **Am I debugging?** Debug on `main` with feature flags, not on a long-lived branch.

If you can't answer any of these, close the branch and start over with a smaller scope. Nothing is lost — the thinking is still in your head.

## Deploy anxiety is a signal

If the thought of deploying makes you nervous, the problem is not the deploy — it's one of these:

- **No rollback path** — fix: make rollback a one-click operation. Vercel has instant rollback. Use it.
- **No monitoring** — fix: add basic error tracking (Sentry free tier) so you'd know if something broke. Then stop worrying about the unknown.
- **Too much at once** — fix: smaller PRs. A 10-line PR can't break that much.
- **No tests on the critical path** — fix: one test on the path you're most afraid of. One. Not a suite.
- **You don't understand the code** — fix: don't ship code you don't understand. See ai-pair-programming.

## Verification

At the end of any working day:

1. Check `git log main` on the deployed branch. Is today's date in the top few commits? If yes, you shipped.
2. Hit your production URL in the browser. Does it load? Yes → you're good.
3. Are there any local uncommitted changes older than today? If yes, decide now: ship or delete.
4. Are there any branches older than 3 days? If yes, either merge, split, or close them.

Do this daily. It takes 60 seconds. It's the single best practice for keeping a solo project alive.
