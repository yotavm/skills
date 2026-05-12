# skills

A small collection of **agent skills** I actually use day-to-day. These follow the open [Anthropic Skills spec](https://github.com/anthropics/skills), so they work across any AI coding agent that reads it — **Claude Code, Codex CLI, Cursor, Copilot, Gemini CLI, Vercel's agent runtime, and others**. Not just Claude.

Public so anyone can install them and so non-technical folks can hand the URL to their agent and have it set them up.

## Install (the easy way — works with any compatible agent)

Open your agent (Claude Code, Codex CLI, Cursor, Copilot, etc.) and paste this:

> Install the skills from `https://github.com/yotavm/skills` for me — copy each folder under `skills/` into my local skills directory (`~/.claude/skills/` for Claude Code, the equivalent location for whichever agent I'm using).

That's it. The agent will clone this repo, copy the skill folders into the right place, and confirm each one is registered. No terminal commands required.

## Install just one skill

Don't want all of them? Paste the URL of the skill you want and ask your agent to install only that one. Each skill folder is a self-contained directory on GitHub, so the URL **is** the install target.

> Install the skill at `https://github.com/yotavm/skills/tree/main/skills/slide-deck` for me — download just that folder and put it at `~/.claude/skills/slide-deck/`.

Swap `slide-deck` in the URL for whichever skill you want. Direct links:

- **slide-deck** → `https://github.com/yotavm/skills/tree/main/skills/slide-deck`
- **idea** → `https://github.com/yotavm/skills/tree/main/skills/idea`
- **create-repo** → `https://github.com/yotavm/skills/tree/main/skills/create-repo`

The agent will fetch only that folder (no need to clone the whole repo).

## Install (the manual way — for people who like terminals)

```bash
git clone https://github.com/yotavm/skills.git /tmp/skills

# Claude Code
cp -r /tmp/skills/skills/* ~/.claude/skills/

# Codex CLI
cp -r /tmp/skills/skills/* ~/.codex/skills/

# Cursor / other agents — drop into your agent's skills directory
```

Or grab just one skill without cloning the whole repo:

```bash
# replace SKILL_NAME with: slide-deck | idea | create-repo
SKILL_NAME=slide-deck
mkdir -p ~/.claude/skills/$SKILL_NAME
curl -sL "https://github.com/yotavm/skills/archive/refs/heads/main.tar.gz" \
  | tar -xz -C ~/.claude/skills/$SKILL_NAME --strip=3 "skills-main/skills/$SKILL_NAME"
```

Restart your agent and the new skills will show up next time you start a thread.

## What's in here

| Skill | What it does | Trigger |
|---|---|---|
| **[slide-deck](skills/slide-deck/)** | Generates a polished HTML slide deck using a fixed design system (container-query layout, blue accent palette, prebuilt components). Same aesthetic every time. | "make slides for X", "create a deck", `/slide-deck` |
| **[idea](skills/idea/)** | Captures ideas as well-formatted markdown files in your personal idea vault. Asks 2 clarifying questions if the idea is sparse. | "idea:", "I want to build X", `/idea` |
| **[create-repo](skills/create-repo/)** | Creates a new GitHub repo via the `gh` CLI, with sensible defaults for visibility, README init, and clone-after-create. | "make a new repo", "spin up a repo", `/create-repo` |

## How a skill works

Each folder under `skills/` is one skill. A skill is just a directory with a `SKILL.md` at the root. The `SKILL.md` frontmatter has a `name` and a `description` — the description is what Claude uses to decide when to invoke the skill, so it's deliberately specific and a bit pushy about its triggers.

Skills can also bundle:
- `assets/` — templates, images, anything the skill copies into outputs
- `references/` — long-form docs the skill loads as needed
- `scripts/` — code the skill runs deterministically
- `evals/` — test cases used while building the skill

The format follows the [Anthropic skills spec](https://github.com/anthropics/skills), which is the de-facto standard adopted across the agent ecosystem (Claude Code, Codex CLI, Cursor, Copilot, Gemini CLI, Vercel's agent runtime, and others). One skill, every agent.

## Contributing / forking

Fork it, add your own skills under `skills/`, and PR or just keep it on your fork. The bar for a skill landing here is: I use it, it's documented, and the trigger description is tight enough not to false-fire.

## License

MIT. See [LICENSE](LICENSE).
