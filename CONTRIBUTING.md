# Contributing

Thanks for considering a contribution. This repo exists to make solo developers more effective — every skill should target a real failure mode solo devs hit.

## Before you open a PR

Read [AGENTS.md](AGENTS.md) first. It covers the skill spec, naming rules, writing style, and philosophy.

## Adding a new skill

1. Create a directory: `skills/<skill-name>/`
2. Create `SKILL.md` with valid frontmatter
3. Keep it under 500 lines — move long references to `skills/<skill-name>/references/`
4. Write in second person, active voice, blunt tone
5. Add an entry to the README's skills table
6. Add the skill to `.claude-plugin/marketplace.json`

## Improving an existing skill

- Clarifications, better examples, new failure modes — PRs welcome
- Major rewrites: open an issue first so we can discuss scope
- Don't add sections just to add sections. If a section doesn't change behavior, cut it.

## Skill quality bar

A skill is ready when:

- **It names a specific failure mode**. "Your API bill triples overnight because you're looping over a paid endpoint in dev" is specific. "Be careful with APIs" is not.
- **It gives concrete, actionable steps**. Not principles — steps.
- **It names actual tools and commands**. No vague "use a caching solution" — say which one and why.
- **It tells you when NOT to use it**. Scope boundaries matter.
- **A solo dev could act on it in under 30 minutes**. If the skill requires a week of work to apply, it's a guide, not a skill.

## Commit messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat: add <skill-name> skill`
- `fix: clarify <topic> in <skill-name>`
- `docs: update README`

## License

By contributing you agree your contributions will be licensed under the [MIT License](LICENSE).
