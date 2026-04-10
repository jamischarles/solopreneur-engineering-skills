---
name: incremental-investment
description: Apply ROI thinking to fixes, automation, and tooling decisions. Use when the user mentions "should I automate this," "let me build a script for this," "this is annoying but only happens sometimes," "internal tool," "dev tooling," "I could build X to solve Y," or when weighing whether a small recurring pain is worth a large one-time fix. Covers the ROI math, the frequency-vs-cost matrix, and the solo-dev bias toward over-investing in tooling for rare pain. For cutting scope on user-facing features, see scope-discipline.
---

You are a solo developer. You have an engineering instinct that says "I see a problem, I should fix it." For user-facing bugs in production, that instinct is correct. For everything else — internal tooling, automation, dev scripts, workflow improvements — that instinct is a trap.

This skill is about applying ROI thinking to the decision of whether to fix something at all. Not "how to fix it" — whether the fix is worth the cost.

## The principle

**Small one-time pain doesn't warrant a large investment to fix it.**

A problem that costs you 10 minutes of annoyance once every six months is not worth a day of work to eliminate. That's 80x underwater. Solo devs constantly make this trade the wrong way because "fixing things" feels productive — even when the math says it isn't.

## The ROI question

Before you build any tooling, automation, or "fix" for a recurring pain, compute the three numbers:

1. **Cost of the pain** — how much time does the current (unfixed) situation cost, per occurrence?
2. **Frequency** — how often does it actually happen? (Not "how often it could happen" — how often it actually does.)
3. **Cost of the fix** — how much time will it take to build the permanent solution, INCLUDING maintenance?

Then:

> **Annual pain = cost × frequency**
> **Break-even horizon = fix cost / annual pain**

If the break-even horizon is longer than a year, don't build the fix. Live with the pain.

### Worked examples

**Example 1: Slow deploy script**

- Pain: deploy takes 5 minutes of waiting, manually
- Frequency: 4 deploys per week = 208 per year
- Annual pain: 5 × 208 = 1040 min/year ≈ 17 hours/year
- Fix cost: 4 hours to write a proper parallel deploy script
- Break-even: 4 / 17 = ~3 months

**Verdict: Build the fix.** Pays back in 3 months.

**Example 2: Occasional SQL debugging**

- Pain: once a month, you manually write a query to inspect the DB. 15 minutes each time.
- Frequency: 12 times per year
- Annual pain: 15 × 12 = 180 min/year = 3 hours/year
- Fix cost: 2 days to build a proper admin dashboard = 16 hours
- Break-even: 16 / 3 = ~5 years

**Verdict: Don't build the fix.** You'd be underwater for 5 years. Keep writing the SQL by hand.

**Example 3: Annoying TypeScript error**

- Pain: 30 seconds of irritation every time you open a specific file
- Frequency: 5 times per week = 260 per year
- Annual pain: 0.5 × 260 = 130 min/year = 2 hours/year
- Fix cost: 1 hour to update the types
- Break-even: 1 / 2 = ~6 months

**Verdict: Build the fix.** Small investment, close-to-break-even in 6 months, plus the irritation is gone.

**Example 4: Writing a script to generate test data**

- Pain: 20 minutes to set up test data manually, when you do it
- Frequency: Twice in the past year (you think)
- Annual pain: 20 × 2 = 40 min/year
- Fix cost: 3 hours to build a proper generator + a schema for it
- Break-even: 180 / 40 = 4.5 years

**Verdict: Don't build the generator.** Keep setting up test data manually. You lose if you write the tool.

## The frequency estimation trap

Solo devs consistently overestimate how often a pain point occurs. You remember the last time it happened vividly, so it feels frequent. In reality, most annoyances happen less than you think.

### How to check real frequency

- Look at your git history for similar tasks in the last 6 months
- Count the number of times in a month you actually hit the pain, going forward
- If you can't remember the last time it happened, it's probably not worth fixing

**Rule: If you can't recall three specific instances from the last 60 days, the frequency is too low to warrant a fix.**

## The maintenance drag you forgot about

When estimating "cost of fix," most solo devs only count the initial build cost. But every tool you build has maintenance:

- When the language/framework upgrades, the tool breaks — you fix it
- When the underlying thing it wraps changes, the tool breaks — you fix it
- When you forget how it works months later, you re-read and re-learn it
- When it hides a bug because of wrong assumptions, you debug it

**Multiply your initial estimate by 1.5-2x to get the real cost of fix.** Maintenance is not zero.

A "fix" that saves you 15 minutes but requires 5 minutes of maintenance per month eats 60 minutes of its own savings each year.

## The solo-dev bias toward over-investing

Why do solo devs build tooling that doesn't pay off?

1. **Tool-building feels like "real work."** Writing a script scratches the engineering itch in a way that "just do the task manually again" doesn't.
2. **One-off tasks feel undignified.** There's a cultural pressure that Real Engineers Automate Everything. This is wrong for solo work.
3. **Future-proofing illusion.** "I'll need this again" is almost always wrong.
4. **Procrastination disguised as productivity.** Building a tool to avoid a hard task is still avoiding the hard task.
5. **Underestimating the fix cost.** Solo devs think "I can build this in an hour." It takes four.

Knowing these biases doesn't eliminate them, but it helps you catch yourself.

## The frequency × cost matrix

A visual rule of thumb:

```
                  Rare pain       Frequent pain
                (< 5x/year)      (> 20x/year)
              ┌──────────────┬──────────────┐
  Small pain  │              │              │
  (< 5 min)   │    IGNORE    │   MAYBE FIX  │
              │              │              │
              ├──────────────┼──────────────┤
   Big pain   │              │              │
  (> 30 min)  │  MAYBE FIX   │   FIX NOW    │
              │              │              │
              └──────────────┴──────────────┘
```

- **IGNORE** — Rare + small. Never worth a fix. Live with it.
- **MAYBE FIX** — One dimension is high, one is low. Do the ROI math.
- **FIX NOW** — Big pain that happens often. Almost always worth fixing.

Most solo-dev "tooling ideas" fall in the IGNORE quadrant and get built anyway. That's where the time leaks.

## When to fix despite bad ROI

There are exceptions. Fix things even when the math says no when:

- **The pain is blocking progress on a real feature.** A 30-minute ordeal that happens only rarely is worth fixing if it stops you from shipping the main thing today.
- **The fix unlocks a new capability.** "Fixing" something that lets you do a NEW kind of work is an investment, not a fix.
- **The current state is dangerous.** If the pain includes "and sometimes it corrupts data" or "and it might fail silently," the fix is worth it for safety, not time savings.
- **You're learning.** Sometimes the tool-building IS the point. But be honest with yourself — that's not ROI, it's education.

## The "cheaper fix" question

Before approving a proposed fix, always ask: **"Is there a cheaper version of this fix that still solves 80% of the pain?"**

Often, a 2-hour fix can be replaced with a 15-minute fix that's ugly but handles the worst 3 cases. The ugly version might be the right call.

### Examples

- **Bad:** Build a proper admin CRUD UI (2 days)
- **Cheap:** Build a single "reset user" form with a password field (1 hour)
- **Verdict:** Cheap version handles the 90% case. Build that.

- **Bad:** Refactor the entire deploy script (1 day)
- **Cheap:** Add one `sleep 5` and a better error message (5 minutes)
- **Verdict:** Cheap version solves the actual problem.

- **Bad:** Build a test data generator system (1 week)
- **Cheap:** Copy-paste a fixture JSON file and call `db.insert()` on it (15 minutes)
- **Verdict:** Cheap version is the right investment level.

## Verification

Before you start building any tool, script, or fix for a recurring pain:

1. **Have you computed annual pain (cost × frequency)?** If no, don't start.
2. **Is the break-even horizon under 1 year?** If no, don't build.
3. **Have you multiplied fix cost by 1.5x for maintenance?** If no, your math is optimistic.
4. **Is there a cheaper version of this fix that solves 80%?** Consider it.
5. **Are you building this to avoid harder, higher-value work?** Be honest.

After the fix is built:

1. **Did you actually save time overall?** Check back in 3 months — did the fix pay off, or did the maintenance drag eat the savings?
2. **Is the fix still in use?** If you built a tool and abandoned it, note that next time you're tempted by a similar idea.
3. **Did the fix enable new work, or just reduce old pain?** The former is investment, the latter is optimization. Different mental categories.

The goal is not to be a tooling purist. It's to stop letting "fixing things" feel productive when it isn't.
