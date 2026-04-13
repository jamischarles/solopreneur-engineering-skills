---
name: impact-scoring
description: Capture all your problems and opportunities, score them, and make a balanced investment decision across limited time. Use when the user says "I don't know what to work on next," "there are too many things to fix," "should I build X or fix Y," "everything feels urgent," or when about to start a sprint or planning week. Covers problem capture, impact/effort scoring, and portfolio-level tradeoffs. For writing clear problem statements before scoring, see problem-first. For shaping the work once you've decided what to build, see task-shaping.
---

You are a solo developer. You have one resource — your time — and it is always less than the list of things you could work on. The question is never "should I work harder?" It's always "which thing is worth doing next?"

This skill gives you a repeatable way to answer that question without agonizing over it each time.

## The core principle: capture before you act

When a problem appears, your instinct is to fix it. Resist this instinct.

**Capture it first. Score it. Then decide.**

This is not bureaucracy. It takes 90 seconds. What it prevents is a life of whack-a-mole — always fixing the loudest problem, never addressing the most important one.

A captured, scored backlog lets you compare apples to apples. Without it, you're comparing "this bug is right in front of me" to "I vaguely recall there's a churn problem" — and the bug wins by default every time, regardless of importance.

## Step 1: Capture everything

Maintain a single list. One place. Not across Notion, Slack threads, browser tabs, and sticky notes.

For each item, capture:

```
Problem: <clear problem statement — see problem-first skill>
Type: bug | friction | opportunity | debt | infrastructure
Who: <who has the problem>
Source: <how you found out — user complaint, data, observation, gut>
```

The problem statement is required. If you can't write it, don't add the item yet — go understand the problem first.

**The list is not a commitment.** Capturing something does not mean you'll build it. It means you won't forget it, and you can evaluate it fairly alongside everything else.

### Types

| Type | Description |
|------|-------------|
| Bug | Something is broken that was working before |
| Friction | Something works but is harder than it should be |
| Opportunity | A new capability that could create value |
| Debt | Something that slows you down internally (code quality, tooling, documentation) |
| Infrastructure | Non-user-facing improvements (reliability, scalability, observability) |

Knowing the type helps you balance the portfolio. If your list is 90% opportunities and 0% bugs, you're probably ignoring user pain. If it's 90% debt, you're probably procrastinating on impact.

## Step 2: Score each item

Score on three dimensions. Each is 1-5.

### Dimension 1: Problem severity (1-5)

How bad is the problem right now, for the person who has it?

| Score | Description |
|-------|-------------|
| 1 | Mild annoyance. Easy workaround exists. Most users don't notice. |
| 2 | Real friction. Some users complain. Workaround is clunky but works. |
| 3 | Significant pain. No good workaround. Affects the primary use case. |
| 4 | Blockers. Users can't complete important tasks. Causes direct loss (churn, revenue, trust). |
| 5 | Critical. Data loss, security issue, complete outage, or existential for the product. |

### Dimension 2: Solution confidence (1-5)

How confident are you that your proposed solution actually solves this problem?

| Score | Description |
|-------|-------------|
| 1 | Guessing. You have no validation that this solution addresses the root cause. |
| 2 | Some signal. One user asked for it, or you have a hypothesis, but no evidence. |
| 3 | Reasonable confidence. Multiple users have this problem and have validated the direction. |
| 4 | High confidence. You've seen users fail and you know exactly what's blocking them. |
| 5 | Certain. You've built this before, or have direct evidence this solution solves the problem. |

**Low solution confidence is a reason to do research, not to skip the problem.** If severity is 4 and confidence is 1, the next step is a spike or user conversation — not picking a different problem.

### Dimension 3: Effort (1-5, inverse — lower effort scores higher)

How much time will this take? Scored so that lower effort gives a higher contribution to the final score.

| Score | Days | Description |
|-------|------|-------------|
| 5 | < 1 day | Tiny — a configuration change, a one-line fix, a copy tweak |
| 4 | 1-2 days | Small — a self-contained change in 1-3 files |
| 3 | 3-5 days | Medium — a multi-file feature or non-trivial refactor |
| 2 | 1-2 weeks | Large — a major feature or significant architectural change |
| 1 | > 2 weeks | Too big — needs to be broken down before scoring |

**If effort scores a 1, stop and re-shape.** A task that takes more than 2 weeks is not a task — it's a project. Break it into shaped pieces that can be individually scored.

### Final score

```
Score = (Problem Severity × Solution Confidence × Effort) / 5
```

Range: 1–25. Higher is better.

| Score | Action |
|-------|--------|
| 20-25 | Do this now. Clear winner. |
| 12-19 | Strong candidate. Include in next cycle. |
| 6-11 | Worth keeping. Schedule if bandwidth allows. |
| 1-5 | Low priority. Revisit quarterly or delete. |

### Example scoring

> **Problem:** Users who finish onboarding don't know their first action should be connecting a data source. 30% drop off before connecting anything.
> **Type:** Friction
> **Severity:** 4 (blocks activation, causes churn)
> **Confidence:** 4 (funnel data confirms the drop, users have asked "what do I do next?")
> **Effort:** 4 (onboarding copy change + one CTA, half a day)
> **Score:** (4 × 4 × 4) / 5 = **12.8** → Strong candidate

> **Problem:** The dashboard sidebar has a slightly inconsistent margin on mobile.
> **Type:** Friction
> **Severity:** 1 (cosmetic, only noticeable if you're looking for it)
> **Confidence:** 5 (it's obviously a pixel issue, easily fixed)
> **Effort:** 5 (10-minute CSS fix)
> **Score:** (1 × 5 × 5) / 5 = **5** → Low priority

The sidebar bug is *easy* — so it would win a whack-a-mole fight every time. But the scoring makes clear it shouldn't.

## Step 3: Make the portfolio decision

Scoring tells you which items are strongest. It doesn't tell you which ones to actually do.

That's a portfolio decision. You're balancing multiple constraints:

### Constraint 1: Type balance

Don't let one type dominate. A healthy solo dev backlog mixes:
- At least one bug fix or friction reduction per cycle (keeps the product from degrading)
- At least one opportunity per cycle (makes the product grow)
- Occasional debt/infrastructure (keeps you fast over time)

If every item in your current cycle is a new feature, your existing users are probably suffering.

### Constraint 2: Dependencies and sequencing

Sometimes a lower-scored item must come first because everything else depends on it. That's okay — use the score to confirm this is worth doing, and note the dependency explicitly.

### Constraint 3: Strategic bets

Some items have outsized potential that the scoring formula doesn't capture. A high-risk, high-reward opportunity might score 8 but still be worth a 2-week bet if it represents a step-change in the product.

**When you override the score, write down why.** "I'm doing this lower-scored item because it's a strategic bet on X." If you can't articulate it in one sentence, you're rationalizing.

### Constraint 4: Your energy and current context

You just spent a week on a complex feature. Your context is narrow, your energy is low. This is a good time to clear a backlog of high-scoring, low-effort items — not to immediately start the next complex feature.

**Work with your energy, not against it.** The score tells you what matters. Your energy state tells you what's executable right now.

## Scoring cadence

| When | Action |
|------|--------|
| Daily | Capture new items. No scoring needed immediately. |
| Weekly | Score recently captured items. Reorder the list. Pick the next cycle's work. |
| Monthly | Prune items with score < 5 that have been sitting for 30+ days. |
| Quarterly | Re-evaluate the strategic bets. What's changed? What's the right 90-day portfolio? |

## The override anti-patterns

You'll recognize these. Name them when you see them.

**"It'll only take 5 minutes"** — the classic Trojan horse. It never takes 5 minutes. It's how low-severity, low-impact items steal an afternoon. If it scored a 3, it still scores a 3 regardless of how fast you think it'll be.

**"A user just asked for this"** — one user request is not evidence of broad severity. It's a data point. Capture it. Score the severity based on evidence, not recency.

**"I'm excited about this one"** — excitement is fine as a tiebreaker between items with similar scores. It is not a substitute for scoring.

**"We need to fix the foundation before we can build anything else"** — sometimes true. Often an excuse to avoid the hard work of shipping something real. Score the foundation work honestly. If it's genuinely blocking everything, that's a severity 4-5. If it's not, ship something.

## Verification

After scoring your list:

1. Is every item backed by a clear problem statement (not a solution)? If no, rewrite the problem statements.
2. Did any item score a 1 on effort (> 2 weeks)? If yes, break it down.
3. Is the portfolio type-balanced? At least one bug/friction and one opportunity in the next cycle?
4. Can you articulate in one sentence why you're doing the top item first?
5. Have you overridden any score? Did you write down why?

After completing a work cycle:

1. Did the score predict the impact? If a 20-point item turned out to be low-value, recalibrate your severity and confidence criteria.
2. Did anything in the backlog become dramatically more or less important? Rescore those items.
3. What did you learn that should change how you capture or score going forward?
