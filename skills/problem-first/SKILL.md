---
name: problem-first
description: Never start building without a clear problem statement. Use when the user says "I want to build X," "we should add Y," "let me just fix Z," "it would be cool if," or any time a solution is named before the problem. Covers writing a clear problem statement, testing whether you actually understand the problem, and the most common traps — solving the visible symptom, solving yesterday's problem, and solutioning by habit. For what to do once you have a clear problem, see impact-scoring and task-shaping.
---

You are a solo developer. Your time is the scarcest resource you have. The fastest way to waste it is to build the wrong thing perfectly — and the primary cause of building the wrong thing is starting from a solution instead of a problem.

This skill is about one rule: **no solution without a clear problem statement first.**

## The failure mode: solutioning without a problem

You've done this. So has every developer.

Someone (usually you) says: "We should add a notification system." That's a solution. Nobody asked "why?" Nobody asked who has this problem, or when it shows up, or how often, or what they do instead. Within 48 hours, someone is building a notification system that no user needs right now.

Or the blinking red light version: a bug appears, it's visible, it feels urgent, you fix it immediately — while five higher-impact problems sit in a list somewhere. The bug was real. But was it the most important thing? You don't know, because you didn't ask.

**A solution is not a strategy.** The instinct to reach for a solution is strong — it feels like progress. But building the wrong thing is negative progress. It consumes time, generates code you have to maintain, and closes off better options.

## The rule

**Before writing a line of code, you must be able to write a clear problem statement.**

If you can't write the problem statement in 2-3 sentences, you don't understand the problem yet. Stop building. Go understand the problem first.

## What a problem statement is

A problem statement answers three questions:

1. **Who has the problem?** (Be specific — "users" is not specific. "A solo developer who just shipped their first SaaS and is trying to get their first paying customer" is specific.)
2. **What is the problem?** (The situation, the gap, the friction — not the solution.)
3. **Why does it matter?** (Consequence of the problem not being solved — churn, revenue loss, time waste, user confusion.)

### Format

> **[Who]** experiences **[what friction or gap]** when **[what context]**, which causes **[what consequence]**.

### Example (bad)

> "We need a notification system."

That's a solution disguised as a problem. There is no who, no friction, no context, no consequence.

### Example (good)

> "Users who have set up recurring automated reports don't know when a report fails silently, which causes them to make decisions on stale data without realizing it."

Now you have a problem. The notification system might be the right solution — or it might not be. You can't evaluate that until you have the problem statement.

## The five-whys test

If your problem statement sounds like a solution, ask "Why?" five times. You will find the real problem.

> "We should add a notification system."
> Why? → "Because users complain they don't know what's happening."
> Why? → "Because reports run in the background and there's no feedback."
> Why? → "Because we never built any async status tracking."
> Why? → "Because we assumed users would check the dashboard."
> Why? → "Because we designed the UI for power users who remember to check — not for occasional users who don't."

The real problem: **the product was designed for a different user than the one using it.** The notification system might fix the symptom. Redesigning the async flow might fix the cause. You don't know which is right until you have the real problem.

## The four traps

### Trap 1: Fixing the blinking red light

The blinking red light demands attention. It's visible. It's in front of you. It feels urgent.

But urgency and importance are not the same thing. The blinking red light might be a cosmetic bug. The five silent issues in your backlog might include a data integrity problem that's been quietly corrupting new signups for a week.

The blinking red light wins because it's easy to point at. The silent issues don't fight back.

**The rule:** Before fixing the blinking red light, ask: "Is this the highest-impact thing I can work on right now?" If you don't know the answer, your first task is to find out — not to start fixing.

### Trap 2: Solving yesterday's problem

You scoped and shaped this feature three weeks ago. The problem was real then. Is it still the right problem now?

Circumstances change. A user complained loudly two weeks ago — but they churned last week. A competitor shipped something that made this problem irrelevant. A data point you received yesterday changes what the highest-leverage intervention is.

**The rule:** Before picking up a task from your backlog, re-read the problem statement. Does it still describe the most important problem? If the problem has changed or resolved, update or remove the task.

### Trap 3: The solution is looking for a problem

You read about a technology, a pattern, an idea. It's interesting. Now you're finding places to apply it.

This is common with LLMs right now. "We should add AI to this." Why? What problem? Who has it? What happens if we don't?

**The rule:** If the solution came first, you are not solving a problem — you are exploring a technology. That's fine, but call it what it is. Exploration is a legitimate activity. Shipping exploration to production is not.

### Trap 4: Solving the symptom, not the cause

The user says "I can't find the button." You make the button bigger.

But the real problem is that the user doesn't understand the workflow. The button is fine. The conceptual model is broken.

You fixed the symptom. The user still can't complete the task. You've made the UI slightly worse (now there's a giant confusing button) and the real problem is still there.

**The rule:** Ask "What is the user actually trying to accomplish?" — not "What did they say?" before deciding what to build.

## The problem statement checklist

Before starting any task, verify:

- [ ] I can write the problem statement in 2-3 sentences
- [ ] The problem statement does not contain a solution (no "so that we can build X")
- [ ] I know specifically who has this problem (not just "users")
- [ ] I know what happens if this problem is not solved
- [ ] I have asked "why?" at least twice and still believe this is the real problem
- [ ] This is still the right problem (the situation hasn't changed since I wrote the statement)

If any box is unchecked, your first task is to understand the problem — not to build.

## Verification

After writing a problem statement:

1. Can someone else read it and describe the user's experience without any additional context? If no, it's not clear enough.
2. Does it contain the word "should," "need to," or a product feature? If yes, it's a solution statement, not a problem statement. Rewrite it.
3. Can you imagine two different solutions that both address this problem? If no, your problem statement is too narrow — you've already constrained the solution space.
4. If you walked away from this task for a week, would the problem statement still make sense when you came back? If no, add context.
