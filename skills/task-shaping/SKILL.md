---
name: task-shaping
description: Shape a piece of work before you start building — define the appetite, sketch the rough solution, flag rabbit holes, and mark no-gos. Use when the user mentions "not sure where to start," "let me plan this," "should I build this," "how long will this take," "I keep getting stuck on," or when about to hand a task to an AI coding agent. Based on Shape Up (Ryan Singer, Basecamp), adapted for solo developers. For cutting scope before shaping, see scope-discipline. For handing shaped work to an agent, see ai-pair-programming.
---

You are a solo developer. The hardest part of any non-trivial task is not writing the code — it's knowing what to build before you start. Solo devs tend to skip this step because there's no PM forcing a spec. Unshaped work is how you end up with half-built features and week-old branches.

This skill adapts Ryan Singer's **Shape Up** methodology to solo work. The core insight: you should never start building something that hasn't been shaped. Shaping is a separate, deliberate activity — not a thought you have while coding.

## What shaping is

Shaping turns a vague idea ("I need to add reviews") into a bounded, well-understood piece of work you can start on Monday and finish by Friday. A shaped task has four components:

1. **Appetite** — how much time you're willing to spend
2. **Solution sketch** — rough shape of what you'll build, not the full design
3. **Rabbit holes** — specific traps you've identified and decided how to avoid
4. **No-gos** — things you've decided are explicitly out of scope

Shaping is not the full design. It's the minimum set of decisions you need to make so that coding doesn't derail.

## 1. Appetite (the time budget comes first)

**Decide how much time you want to spend on this BEFORE you decide what to build.** This is counterintuitive and critically important.

The traditional approach: figure out what to build, estimate how long it'll take. The Shape Up approach: decide how long you're willing to spend, then figure out what you can build in that time. The budget comes first, the scope conforms.

### Appetite buckets for solo work

| Appetite | Time budget | Use for |
|----------|-------------|---------|
| Tiny | Half a day | Bug fixes, small UX improvements, config changes |
| Small | 1-3 days | Self-contained feature additions |
| Medium | 1 week | Multi-file features that touch several concerns |
| Big | 2 weeks | Major new capability or significant refactor |
| Too big | > 2 weeks | Stop. Re-shape into smaller pieces. |

**Anything beyond 2 weeks is not shaped — it's an aspiration.** If you can't see the end from the start, cut until you can.

### The appetite question

Before shaping a task, ask: **"How much is this idea worth to me?"**

If the answer is "a week at most," then you're shaping a week-sized solution. Not a month-sized solution you'll "try to do in a week." A week-sized solution is a different design than a month-sized one — you'd cut features, accept constraints, and pick different tools.

The appetite protects you from overbuilding. It forces tradeoffs upfront instead of letting them ambush you on day 5 of a week you thought was going to be day 3.

## 2. Solution sketch (rough, not final)

Sketch enough of the solution to know it will work. No more.

### What a sketch is

A sketch is:
- Rough page layouts (boxes and labels, not pixel-perfect designs)
- Database table names and key columns (not full schemas)
- API endpoint names and inputs/outputs (not full specs)
- The main path through the code (not every branch)
- A short list of files you'll touch (not every function)

### What a sketch is NOT

A sketch is not:
- Pixel-perfect mockups
- Every edge case
- Final naming
- Full error handling
- Performance considerations
- Tests

You are answering "is this doable in the appetite?" — not "what does every detail look like?"

### The "could I start Monday?" test

A good sketch is one where a future version of you, starting on Monday morning, could begin coding immediately without having to make any more big decisions. If there are still unanswered architectural questions, keep sketching until they're resolved.

If after an hour of sketching you STILL have big unanswered questions, that's a signal this work is too risky for its appetite. Either increase the appetite, cut scope, or cancel.

## 3. Rabbit holes (things that will eat you alive)

**A rabbit hole is a place where you could plausibly spend more time than the entire appetite.** Shaping is not complete until you've identified the rabbit holes and decided, in advance, how you'll avoid them.

### Common solo-dev rabbit holes

- **Refactoring adjacent code** — "while I'm in here..." — decide upfront: not touching it
- **Auth edge cases** — decide upfront which auth states you'll support and which you'll punt on
- **Third-party API integration quirks** — time-box the spike; if the API fights back, cut the feature
- **Realtime features** — decide if eventual consistency is acceptable; if yes, save days of work
- **Mobile responsiveness** — decide the minimum breakpoint set; usually desktop + one mobile size is enough for v1
- **Browser compatibility** — decide the minimum supported browsers; resist old-IE support
- **Data migrations** — design the migration, write it last; always leaves rollback path
- **Performance optimization** — decide the acceptable latency threshold; stop optimizing once you hit it

### How to document a rabbit hole

For each rabbit hole you identify, write one line: **"What I'll do instead."**

> "Rabbit hole: auth. What I'll do instead: only support email/password login, no social auth, no password reset in v1."

> "Rabbit hole: real-time updates. What I'll do instead: polling every 30s, no WebSockets."

> "Rabbit hole: the existing notes system needs a refactor. What I'll do instead: add the new feature alongside, don't touch the existing code."

These lines prevent you from falling in. When you hit the rabbit hole mid-build, your shaping notes tell you "don't go down there, remember?"

## 4. No-gos (things you explicitly won't do)

**No-gos are a list of features or capabilities you could include but have decided not to.** They're written DOWN so they're not re-litigated mid-build.

### Why no-gos matter for solo devs

Solo devs constantly re-negotiate scope with themselves. "Should I also add...?" "What if I throw in...?" "It would be cool if..." Every one of these is a scope leak. A written no-go list gives you something to point at when midnight-you wants to add one more thing.

### Example no-gos

For a "reviews feature" task with a 1-week appetite:

> **No-gos:**
> - No ratings (stars, thumbs, etc.) — just text reviews
> - No editing or deleting after submission — if needed, do it manually in the DB
> - No moderation queue — trust all submissions for v1
> - No email notifications on new reviews
> - No "helpful / not helpful" voting
> - No threading or replies
> - No pagination — show the 50 most recent, that's it

Look at that list. Half of these are the things you'd normally add by instinct. Writing them as no-gos makes it deliberate, not accidental.

## The shaping document

A complete shaped task should fit on one page. Structure:

```markdown
# <Task name>

## Appetite
<time budget>

## Problem
<what user need this solves, in 1-3 sentences>

## Solution sketch
<rough layout / flow / key decisions — 1 paragraph or a few bullets>

## Rabbit holes
- <rabbit hole>: <what I'll do instead>
- <rabbit hole>: <what I'll do instead>

## No-gos
- <thing I could do but won't>
- <thing I could do but won't>
```

That's it. If the doc gets longer than one page, either (a) the task is too big, or (b) you're designing, not shaping. Stop and cut back.

## Shaping for AI pair programming

Shaped work is the ideal input for an AI coding agent. The shaping document gives the agent:

- **Explicit scope bounds** — it won't drift into no-gos
- **Known traps** — you've already pre-committed to the workarounds
- **A budget** — you can tell the agent "this should be a 2-hour task" and it can flag if the work is bigger
- **A sketch** — concrete enough to guide, loose enough to let the agent fill in

When you shape a task, you're doing the one thing an AI agent genuinely can't do for you: make the tradeoff decisions that bind what the work becomes. Once those decisions exist in writing, the agent can do the rest of the work faster than you can.

Hand the shaping doc to the agent as the first message in your session. See **ai-pair-programming** for what to do next.

## When NOT to shape

Shaping has overhead. Don't shape:

- **Bugs you can fix in 10 minutes** — just fix them
- **Typos, small copy changes, CSS nudges** — just do it
- **Well-worn patterns you've built before** — muscle memory doesn't need a doc
- **Pure exploration** — when you're learning, not building, a fixed appetite can kill curiosity

Everything else — anything that will take more than a few hours, any work you'd hand to an AI agent, any feature with uncertain scope — should be shaped before coding starts.

## Verification

Before you start coding on any shaped task:

1. **Did you write down the appetite?** If no, you haven't shaped — you've just daydreamed.
2. **Can you describe the solution in one paragraph?** If no, keep sketching.
3. **Have you listed at least 2 rabbit holes with their workarounds?** If you can't think of any, you haven't looked hard enough.
4. **Have you listed at least 3 no-gos?** The fact that you had to decide against them is proof you considered the space.
5. **Does the whole shaping doc fit on one page?** If no, cut.

After the task is done:

1. **Did you stay within the appetite?** If no, why — rabbit hole you missed? Scope creep?
2. **Did any no-gos try to sneak back in?** How did you resist them?
3. **Did any new rabbit holes appear mid-build?** Add them to next time's shaping checklist.

## Reference

This skill is a solo-dev adaptation of **Shape Up** by Ryan Singer (Basecamp, 2019). Full book free online: https://basecamp.com/shapeup. If you want the deeper theory — cool-down periods, betting tables, hill charts — read the book. This skill is the practical solo-dev subset.
