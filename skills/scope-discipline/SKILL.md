---
name: scope-discipline
description: Cut scope before adding it. Use when the user mentions "MVP," "feature creep," "should I add," "nice to have," "refactor first," "framework," "abstraction," "while I'm in here," "configurable," "flexible," or when a task is growing bigger than planned. Covers saying no to features, resisting premature abstraction, and the solo-dev tendency to build the framework before the product. For shipping cadence, see ship-daily.
---

You are a solo developer. You have no teammate to say "wait, do we really need that?" — so you have to be that person for yourself. The hardest engineering skill for solo devs is not writing code. It's deciding what NOT to build.

This skill is about saying no — to features, to abstractions, to configurability, to "while I'm in here" refactors. Every "no" is a deposit in the bank of shipping velocity. Every "yes" is a withdrawal.

## The solo-dev scope trap

The pattern goes like this:

1. You want to build a simple feature
2. You notice the code could be cleaner
3. You start a refactor
4. The refactor needs a new abstraction to be clean
5. The abstraction has edge cases
6. You handle the edge cases
7. Two weeks pass
8. The original feature is not shipped
9. You have no users to give you feedback on the feature you didn't ship
10. The whole thing was a waste

The trap is seductive because every individual step feels like "doing it right." The trap is not the individual decisions — it's the cumulative trajectory. You zoom out and you've built nothing users can use.

## The cut-first principle

**For any feature, your first pass is a scope cut, not a design.**

Before asking "how should this work?" ask "what's the smallest version that provides value?"

### Scope-cutting questions

Ask these in order. Stop at the first "no."

1. **Can it be a static page instead of dynamic?** (Static is 10x cheaper than dynamic.)
2. **Can it be manual instead of automatic?** (You running a script once a week is fine for a solo project.)
3. **Can it be Stripe Payment Links instead of a custom checkout?** (Same for every third-party integration — use the hosted version.)
4. **Can it be a CSV export instead of a dashboard?** (You looking at a spreadsheet is sometimes the MVP.)
5. **Can it be an email instead of a feature?** (Notification features often work fine as a daily email you send yourself.)
6. **Can one user have it instead of all users?** (Hardcode yourself as the only user for v0.)
7. **Can it be no feature at all?** (Most proposed features have no users yet.)

Each of these converts a week of work into a day of work.

## Premature abstraction — the silent killer

The rule: **three similar pieces of code is better than a premature abstraction.**

Abstractions are not free. Every abstraction has:

- A name you must remember
- A mental model you must keep current
- An interface that constrains future changes
- Tests that must be maintained
- Edge cases that must be considered
- A failure mode when the abstraction doesn't fit the next use case

An abstraction is only worth the cost when you have seen at least three concrete use cases and you understand how they really differ. Before that, duplication is cheaper.

### Signs you're abstracting too early

- "This might be useful later"
- "What if we need to support X?"
- "Let me make this configurable"
- "I'll just add one more parameter"
- "We could extend this for..."
- "This should be a library / plugin / factory / provider"

All of these are speculation. Solo devs don't have budget for speculation. Build for what you know you need.

### The Rule of Three

1. First time: write it inline
2. Second time: copy-paste it (yes, really)
3. Third time: NOW extract an abstraction — but only if the three sites are genuinely the same

The copy-paste step feels wrong. It's correct. The second site often reveals that sites one and two are not the same problem, and the "abstraction" you would have built would have been wrong.

## "While I'm in here" refactors

The second-deadliest phrase in solo dev, after "it should be easy."

You open a file to fix a typo and notice the code is messy. You start cleaning up. Three hours later the typo fix is still unshipped and you have a 400-line refactor with its own bugs.

### The rule

**When you're in a file to do task A, do task A and nothing else.**

Refactors are their own PRs. If the code bothers you, create a note ("TODO: refactor X when there's time") and come back in a dedicated session. Never mix a refactor with a feature.

### Exceptions

- Renaming one variable to something clearer (seconds, not minutes)
- Deleting one obviously dead line
- Fixing one obviously broken type annotation that blocks your actual change

Anything larger becomes a separate PR.

## Configurability is a liability

**Every config option is a future bug report.**

If you add a setting, you must:
- Document it
- Test all combinations
- Debug when users misconfigure it
- Migrate it when the config format changes
- Maintain backward compatibility when you want to change the default

Solo projects cannot afford this. Default to hardcoded values until a real user tells you the default is wrong. Then consider adding the option — or just changing the default, which is often the right move.

### Config you probably don't need

- A "theme" setting in v0
- An "advanced" settings panel
- Environment-specific feature flags for a single-environment project
- Per-user preferences for things you could make one good default for
- A "plugins" system before you have users
- An "API" before the product works

### Config you actually need

- Environment variables for secrets and per-environment URLs
- Feature flags for genuinely in-progress work you want to ship dark
- A way to toggle mocks in development (see cost-control)

That's almost all of it.

## Framework-building disease

The solo dev tendency to build the framework before the product. Symptoms:

- You've spent more than a day on "project setup" without writing a user-facing feature
- You have a `core/` or `shared/` or `lib/` directory that's bigger than your feature code
- You're designing a plugin architecture
- You're building a "CLI tool" for your own project
- You've said "first I'll build the engine, then the game"

The cure: **work backwards from a deploy.** What is the user-visible thing you want to ship today? Build directly to that. The framework can emerge from duplication later. It almost never emerges well from speculation up front.

## Saying no to yourself

The hardest conversations are with yourself at midnight when you want to "just add one more thing."

### The overnight test

If you come up with a new feature idea, do not add it to the in-progress branch. Write it down. Read it again in the morning.

Half the time, morning-you will say "why was I going to build that?" Half the time, morning-you will say "yeah, that's worth its own PR." Both outcomes are better than midnight-you merging scope creep.

### The "next user" test

When tempted to add a feature, ask: **"Will my next user leave if this feature isn't here?"**

Not "would it be nice if..." Not "could someone want..." Would they leave?

If no, don't build it. Put it in a `maybe.md` file. If three real users ask for it, then build it.

## Verification

After every session, ask:

1. **Did I ship the feature I planned, or something different?** If different, was the change intentional?
2. **Did I create any new abstractions?** If yes, did I have three concrete use cases?
3. **Did I add any configurability?** If yes, did a real user ask for it?
4. **Did I do any "while I'm in here" refactoring?** If yes, should that have been a separate PR?
5. **How much of today's code is user-visible?** If < 20%, you may be building the framework instead of the product.

Scope discipline is a daily practice. The moments where you say "no" to yourself are the moments where the project stays alive.
