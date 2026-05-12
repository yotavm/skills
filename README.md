# skills

A small collection of [Claude Code](https://claude.com/claude-code) skills I actually use day-to-day. Public so anyone can install them and so non-technical folks can hand the URL to Claude and have it set them up.

## Install (the easy way — works on any machine with Claude Code)

Open Claude Code and paste this:

> Install the skills from `https://github.com/yotavm/skills` for me — copy each folder under `skills/` into `~/.claude/skills/`.

That's it. Claude will git-clone this repo, copy the skill folders into your local `~/.claude/skills/`, and confirm each one is registered. No terminal commands required.

## Install (the manual way — for people who like terminals)

```bash
git clone https://github.com/yotavm/skills.git /tmp/skills
cp -r /tmp/skills/skills/* ~/.claude/skills/
```

Restart Claude Code and the new skills will show up next time you start a thread.

## What's in here

| Skill | What it does | Trigger |
|---|---|---|
| **[slide-deck](skills/slide-deck/)** | Generates a polished HTML slide deck using a fixed design system (container-query layout, blue accent palette, prebuilt components). Same aesthetic every time. | "make slides for X", "create a deck", `/slide-deck` |
| **[idea](skills/idea/)** | Captures ideas as well-formatted markdown files in your personal idea vault. Asks 2 clarifying questions if the idea is sparse. | "idea:", "I want to build X", `/idea` |
| **[create-repo](skills/create-repo/)** | Creates a new GitHub repo via the `gh` CLI, with sensible defaults for visibility, README init, and clone-after-create. | "make a new repo", "spin up a repo", `/create-repo` |
| **[summer-pr-skill](skills/summer-pr-skill/)** | Opens pull requests in the Summer monorepo following the team's PR conventions. (Personal-workflow skill — likely only useful if you work at Summer.) | "open a PR", "pr this" |

## How a skill works

Each folder under `skills/` is one skill. A skill is just a directory with a `SKILL.md` at the root. The `SKILL.md` frontmatter has a `name` and a `description` — the description is what Claude uses to decide when to invoke the skill, so it's deliberately specific and a bit pushy about its triggers.

Skills can also bundle:
- `assets/` — templates, images, anything the skill copies into outputs
- `references/` — long-form docs the skill loads as needed
- `scripts/` — code the skill runs deterministically
- `evals/` — test cases used while building the skill

The format follows the [Anthropic skills spec](https://github.com/anthropics/skills). Skills are portable across Claude Code, Codex CLI, and other agents that read the same spec.

## Contributing / forking

Fork it, add your own skills under `skills/`, and PR or just keep it on your fork. The bar for a skill landing here is: I use it, it's documented, and the trigger description is tight enough not to false-fire.

## License

MIT. See [LICENSE](LICENSE).
