---
name: create-repo
description: >
  Use this skill whenever the user wants to create a new GitHub repository using the `gh` CLI.
  Trigger on phrases like "create a repo", "make a new repo", "new github repo", "create a github
  project", "set up a repo", "init a repo", "new private repo", "spin up a repo", or any variation
  of creating a GitHub repository. Always use this skill when repo creation is the goal — even if
  the user doesn't say "gh" or "GitHub" explicitly.
---

# create-repo: Create GitHub Repositories

This skill creates a GitHub repository via the `gh` CLI. It works in both **Cowork** (using
`AskUserQuestion`) and **Claude Code / terminal** (using `gh repo create` interactively or with
flags).

## Prerequisites

- `gh` CLI must be installed and authenticated (`gh auth status`)
- For org repos: the user must have permission to create repos in the org

---

## Workflow

### Step 1: Gather inputs

Use `AskUserQuestion` to collect the following (ask all in one call):

1. **Repo name** — what to call the repository (e.g. `my-app`, `cool-backend`)
2. **Owner** — personal account or an org the user belongs to
3. **Visibility** — public or private
4. **Description** — optional, short one-liner describing the repo
5. **Initialize with README?** — yes/no
6. **Clone locally after creation?** — yes/no

Ask these as multiple questions in a single `AskUserQuestion` call. If you're in a terminal/console
context and the user has already provided some of these details in their message, skip asking for
those and just confirm before running.

### Step 2: Build and run the `gh` command

Construct the command based on the answers:

```bash
gh repo create [OWNER/]REPO_NAME \
  --public | --private \
  [--description "..."] \
  [--add-readme] \
  [--clone]
```

**Owner logic:**
- Personal account: `gh repo create REPO_NAME ...` (no org prefix needed)
- Org repo: `gh repo create ORG_NAME/REPO_NAME ...`

**Examples:**

Personal, private, with README, cloned locally:
```bash
gh repo create my-app --private --add-readme --clone
```

Org repo, public, with description:
```bash
gh repo create my-org/cool-backend --public --description "Backend API for the cool app" --add-readme --clone
```

### Step 3: Confirm and run

Before executing, show the user the exact command that will be run and ask for confirmation. Then
run it via `Bash`.

After success, show the user:
- The new repo URL (from `gh repo create` output)
- The local path if cloned

---

## Error handling

- If `gh` is not installed: tell the user to install it via `brew install gh` (macOS) or the
  [official docs](https://cli.github.com/)
- If not authenticated: run `gh auth login` and guide the user through it
- If the repo name already exists: suggest a different name or ask if they meant to push to it
- If org permissions are denied: explain they may need to be added to the org first

---

## Notes

- Repo names should use kebab-case (e.g. `my-cool-app`, not `my_cool_app` or `MyCoolApp`)
- `--add-readme` initializes the repo with a README so it can be cloned immediately
- `--clone` will clone the repo into the current working directory
- If the user wants to push an existing local project, suggest: `gh repo create` in interactive
  mode, or create the remote and then `git remote add origin` + `git push`
