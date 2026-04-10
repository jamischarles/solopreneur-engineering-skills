---
name: ai-pair-programming
description: Work effectively with AI coding agents (Claude Code, Cursor, Codex, Windsurf). Use when the user mentions "Claude Code," "Cursor," "AI agent," "vibe coding," "the agent keeps," "it's not doing what I want," "over-delegating," or when the user is deciding how much to let the agent drive vs. drive themselves. Covers task scoping, review discipline, knowing when to take the wheel, and shipping code you understand. For cost control on AI API bills, see cost-control.
---

You are a solo developer using AI coding agents as your pair programmer. The agent is your multiplier — it lets you ship faster than any solo dev five years ago could have imagined. It is also the fastest way to generate 5,000 lines of code you don't understand, can't debug, and can't safely change.

This skill is about getting the multiplier without the debt.

## The three modes of AI pair programming

At any moment, you are in one of three modes. Knowing which one is half the battle.

### Mode 1: Driver

**You are writing the code. The agent is a reference and a typing assistant.**

- You know exactly what you want
- You already understand the problem
- The agent fills in boilerplate, suggests names, catches typos
- You read every line the agent writes before accepting it

Use this mode for critical paths, security-sensitive code, novel architecture, and anything you'll have to debug in six months.

### Mode 2: Navigator

**The agent is writing the code. You are steering, reviewing, and course-correcting.**

- You know what you want but not exactly how
- The task is well-scoped but involves several files
- You read the agent's plan before it acts
- You read every diff before committing

Use this mode for: feature implementation in well-understood domains, refactors, test writing, migrations, boilerplate-heavy additions.

### Mode 3: Delegated

**The agent is doing something end-to-end. You check the result.**

- The task is research or exploration, not production code
- OR the task is reversible and low-stakes
- OR the task is pure boilerplate (e.g. "add these five fields to the schema and the form")

Use this mode for: finding things in the codebase, generating fixtures, running tests, exploring API docs, renaming a variable across files, writing throwaway scripts.

**The most common mistake is running Mode 3 on Mode 2 work.** You delegate "build the auth flow" and get back 800 lines you've never seen. Now you own code you don't understand and can't safely change. The feature ships. Six weeks later you need to change it. You can't.

## Task scoping rules

Before you start an AI session, decide:

1. **What is the concrete output?** "A feature" is not scoped. "A PR that adds a `/api/reviews` endpoint with POST and GET, using the existing auth middleware, with one integration test for each" is scoped.
2. **Which mode am I in?** If it's Mode 2 or 3, scope shrinks until it fits in one readable diff.
3. **What's my review budget?** If you can't review 500 lines right now, don't let the agent write 500 lines.
4. **What does done look like?** Define it before starting. "It works" is not done; "the test passes AND I've read the diff AND it's deployed to preview" is done.

## Scoping by size

| Task size | Mode | Review before commit |
|-----------|------|---------------------|
| < 50 lines | Any | Yes, every line |
| 50-200 lines | Navigator or Driver | Yes, every line |
| 200-500 lines | Navigator | Yes, diff-by-diff |
| > 500 lines | Split into smaller tasks first | N/A |

**If the agent wants to write > 500 lines for a single task, stop and re-scope.** Either the task is too big, or it's doing work you didn't ask for.

## Prompting for good output

Good prompts look more like specifications than conversations.

### Bad prompt
> "Can you add a reviews feature?"

### Good prompt
> "Add a `reviews` resource following the pattern in `notes/` — Supabase table, GET/POST/PUT/DELETE routes in `api/reviews/`, matching types in `lib/types.ts`. Use the existing `requireAuth` middleware. Don't touch the dashboard UI — that's a separate task."

The good prompt gives the agent:
- A specific pattern to follow (so it doesn't invent a new one)
- Explicit file paths (so it doesn't guess)
- A scope boundary (so it doesn't drift)
- What NOT to do (often more valuable than what TO do)

### Prompting for exploration

When you don't know the codebase yet, prompt for research first, then action:

> "First, tell me how notes are implemented: the table schema, the API routes, and where the UI calls them. Don't write any code yet."

Read the answer. Then prompt for the action.

## Review discipline

The non-negotiable rule: **you don't commit code you haven't read.** Not "skimmed." Read.

### The 60-second review

For every diff, ask:

1. **Does it do what I asked?** Not "does it look impressive" — does it solve the specific problem?
2. **Does it add code I didn't ask for?** Drive-by refactors, extra abstractions, new files — these are scope creep.
3. **Does it use existing utilities?** Or did it reinvent a helper that already exists?
4. **Would I be able to debug this?** If something breaks at 2am, will you understand the code?
5. **Is there anything I don't understand?** Stop. Ask the agent to explain it, or rewrite it yourself in Mode 1.

If the answer to #5 is yes, you are about to commit a landmine.

### Red flags in AI-generated code

- New dependencies added to `package.json` that you didn't approve
- New config files (eslint, prettier, tsconfig variations) that weren't requested
- Mock data left in production paths ("TODO: remove")
- `try/catch` blocks that swallow errors silently
- "Defensive" null checks for cases that can't happen
- Unused imports or dead code "for future use"
- Over-engineered abstractions for single use cases
- Tests that assert on implementation details instead of behavior

Each of these is a signal the agent did more than you asked. Push back.

## When to take the wheel

Switch to Mode 1 (Driver) when:

- The agent has tried and failed twice on the same bug
- You realize you don't understand what the agent is doing
- The code is in a security-sensitive path (auth, payments, PII)
- You're about to commit something that scares you
- The diff is getting bigger than the original scope
- You're tired — tired solo devs should not delegate to AI

## Over-delegation recovery

You've already let it happen. The codebase has 2,000 lines of AI-generated code you don't fully understand. What now?

1. **Stop adding features until you understand what exists.** New code on a foundation you don't understand compounds the debt.
2. **Pick one module. Read it end-to-end.** Rewrite anything confusing. The rewrite is cheaper than the debugging session at 2am.
3. **Write one integration test per critical path.** This forces you to understand the happy path.
4. **Delete aggressively.** If the agent added a feature you don't need or don't remember asking for, remove it.
5. **From now on, review every diff.** No exceptions.

## Verification

After any AI-assisted session:

1. Can you explain the change in one sentence to yourself? If no, you don't understand it.
2. Can you list the files changed from memory? If no, the session was too big.
3. Is there anything in the diff you didn't explicitly ask for? Remove it or understand why it's needed.
4. Does the feature do what the original prompt asked — and nothing more?
5. If a test broke, do you know WHY, or just that it broke?

A clean AI session ends with all five answered "yes." A sloppy one ends with you hoping it works.
