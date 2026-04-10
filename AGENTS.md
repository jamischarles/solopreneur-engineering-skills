# AGENTS.md

Guidelines for AI agents working in this repository.

## Repository Overview

This repo contains **Agent Skills** for solo developers. Skills follow the [Agent Skills specification](https://agentskills.io/specification.md) and install to `.claude/skills/` (or `.agents/skills/` for cross-agent compatibility).

This repo also serves as a **Claude Code plugin marketplace** via `.claude-plugin/marketplace.json`.

- **GitHub**: [jamischarles/solopreneur-engineering-skills](https://github.com/jamischarles/solopreneur-engineering-skills)
- **License**: MIT

## Repository Structure

```
solopreneur-engineering-skills/
├── .claude-plugin/
│   └── marketplace.json    # Claude Code plugin marketplace manifest
├── skills/                 # Agent Skills
│   └── skill-name/
│       └── SKILL.md        # Required skill file
├── AGENTS.md               # This file
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Agent Skills Specification

### Required frontmatter

```yaml
---
name: skill-name
description: What this skill does and when to use it. Include trigger phrases.
---
```

### Frontmatter rules

| Field | Required | Constraints |
|-------|----------|-------------|
| `name` | Yes | 1-64 chars, lowercase `a-z`, numbers, hyphens. Must match directory name exactly. |
| `description` | Yes | 1-1024 chars. Describe what it does AND when to use it. Include trigger phrases. |
| `license` | No | Default MIT |

**Name rules**: lowercase letters, numbers, hyphens only. Cannot start/end with hyphen. No `--`.

- Valid: `cost-control`, `ship-daily`, `ai-pair-programming`
- Invalid: `Cost-Control`, `-cost`, `cost--control`

### Description best practices

The `description` drives skill discovery. Include:
1. What the skill does
2. When to use it (trigger phrases the user might say)
3. Related skills for scope boundaries

Example:
```yaml
description: Stop hammering paid APIs during development. Use when the user mentions "API costs," "rate limits," "caching," "mocking," or when working on anything that calls a paid third-party service. For shipping discipline, see ship-daily.
```

## Writing Style

### Structure
- `SKILL.md` under 500 lines (move details to `references/` if needed)
- H2 (`##`) for main sections, H3 (`###`) for subsections
- Short paragraphs (2-4 sentences)
- Bullet points and tables liberally

### Tone
- Direct and instructional
- Second person ("You are a solo developer...")
- No corporate speak. No excessive hedging. No "we" — use first person or neutral voice.
- Professional but blunt. Solo devs don't have time for throat-clearing.

### Formatting
- Bold (`**text**`) for key terms
- Code blocks for examples
- Tables for reference data
- No emojis unless the user asks for them

### Clarity
- Clarity over cleverness
- Specific over vague (name actual tools, actual commands)
- Active voice
- One idea per section

## Philosophy

These skills exist because solo developers break in predictable ways:

1. **Cost blowouts** — calling paid APIs in dev loops without caching
2. **Shipping paralysis** — long branches, perfectionism, deploy anxiety
3. **Scope creep** — building the framework before the product
4. **Over-delegation to AI** — shipping code you don't understand

Every skill should target a real failure mode, not a hypothetical one. If you can't name a specific way a solo dev gets burned without this skill, the skill shouldn't exist.

## Git Workflow

### Branch naming
- New skill: `feature/skill-name`
- Improvement: `fix/skill-name-description`
- Docs: `docs/description`

### Commits
Follow [Conventional Commits](https://www.conventionalcommits.org/):
- `feat: add cost-control skill`
- `fix: clarify mock layer guidance in cost-control`
- `docs: update README`

### PR checklist
- [ ] `name` matches directory name exactly
- [ ] `name` follows naming rules
- [ ] `description` includes trigger phrases
- [ ] `SKILL.md` under 500 lines
- [ ] No credentials or sensitive data
- [ ] At least one concrete, actionable recommendation per section
