# Solopreneur Engineering Skills

A collection of AI agent skills for solo developers shipping real software. Scoped to **vibe coding as a solo dev** — the actual engineering problems you hit when you're the whole team: controlling costs, shipping daily, working effectively with AI coding agents, and keeping scope honest.

Built for Claude Code, Cursor, Codex, Windsurf, and any agent that supports the [Agent Skills spec](https://agentskills.io).

## What are Skills?

Skills are markdown files that give AI agents specialized knowledge and workflows for specific tasks. Drop them into your project and your agent will recognize when you're working on a solo-dev problem and apply the right playbook.

## Available Skills

| Skill | Description |
|-------|-------------|
| [ai-pair-programming](skills/ai-pair-programming/) | Work effectively with AI coding agents — scoping tasks, reviewing output, avoiding over-delegation, knowing when to drive vs. let the agent drive. |
| [cost-control](skills/cost-control/) | Stop hammering paid APIs during development. Mock layers, persistent caching, rate limiting, and "is this call necessary?" instincts for solo budgets. |
| [scope-discipline](skills/scope-discipline/) | Say no to features. Ship MVPs. Cut before adding. Resist the solo-dev tendency to build the framework before the product. |
| [ship-daily](skills/ship-daily/) | Ship something every day. Small PRs, continuous deployment, deploy-first mindset. Avoid long branches and pre-launch perfectionism. |

## Installation

### Claude Code plugin marketplace

```bash
/plugin marketplace add jamischarles/solopreneur-engineering-skills
/plugin install solopreneur-engineering-skills
```

### Clone and copy

```bash
git clone https://github.com/jamischarles/solopreneur-engineering-skills.git
cp -r solopreneur-engineering-skills/skills/* .claude/skills/
```

### Git submodule

```bash
git submodule add https://github.com/jamischarles/solopreneur-engineering-skills.git .claude/solopreneur-engineering-skills
```

## Who this is for

- Solo founders shipping SaaS
- Indie hackers with a side project that pays rent
- Anyone who codes alone and feels the weight of every decision
- People who use Claude Code / Cursor / Codex daily and want their agent to behave like an experienced collaborator

## Philosophy

Solo dev work breaks in specific, predictable ways:

1. **You run up a bill you didn't expect** — unmocked API calls in a dev loop
2. **You stop shipping** — branches pile up, perfectionism takes over, deploys get scary
3. **You build the wrong thing** — scope creeps, abstractions appear before users do
4. **You let the AI drive into a wall** — over-delegating, shipping code you don't understand

Every skill in this repo targets one of these failure modes.

## Contributing

Found a way to improve a skill or have a new one to add? [Open a PR](CONTRIBUTING.md).

## License

[MIT](LICENSE) — use these however you want.
