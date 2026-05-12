---
name: summer-pr-skill
description: How to open a pull request in the Summer monorepo using the GitHub CLI (`gh pr create`). Use this skill whenever working in /Users/yotavmasa/summer/repo/summer and the user asks to open, create, or submit a PR, push a branch for review, or make a pull request. Also trigger when the user says things like "open a PR", "create a PR", "submit for review", "make a pull request", or "pr this" — even if they don't explicitly mention GitHub CLI or Summer.
---

# Opening a PR in the Summer repo

## Dependency: GitHub CLI

The `gh` CLI must be installed and authenticated. Check with:

```bash
gh auth status
```

If not installed:

```bash
# macOS
brew install gh

# then authenticate
gh auth login
```

---

Summer has specific PR conventions enforced by GitHub Actions. Getting the title and body right matters — the labeler workflow auto-populates the Shortcut story link from the branch name, but only if the format is correct.

## Step 1: Determine the PR title

Format: `<type>(<scope>): [sc-<ticket#>] <subject>`

**Rules:**
- Subject must be **50 characters or fewer**
- If there's no Shortcut ticket, use `sc-n/a`
- Extract the ticket number from the branch name — branches follow `sc-<number>/...` or `ch-<number>/...`

**Types:** `feature`, `fix`, `docs`, `style`, `refactor`, `chore`

**Scopes:** `auth`, `app`, `deps`, `onboarding`, `rec-engine`, `idr`, `pslf`, `refi`, `overpayments`, `guides`, `loans`, `shared`, `tooling`, `ci`, `analytics`, `forgiveness-finder`, `csp`, `wizard`, `covid`, `partner`, `cashback`, `default`, `private-loans`, `eslc`, `paywall`, `biden`, `tuition`, `mosaic`, `ea`, `rm`, `clerk`, `server`, `portal`

**Examples:**
- `feature(idr): [sc-20032] Add new pay frequency question`
- `fix(refi): [sc-18450] Correct APR calculation on summary page`
- `chore(tooling): [sc-n/a] Upgrade ESLint to v9`

## Step 2: Write the PR body

Use this exact template (it matches `.github/pull_request_template.md`):

```
_Shortcut:_

## What

<Describe the problem this PR solves>

## Why

<Explain why this work is necessary>

## How

<Describe what was actually changed. Include before/after screenshots for UI changes>

## How to Test

<Steps to verify the work>
```

The `_Shortcut:_` line is intentionally bare — GitHub Actions automatically replaces it with the full Shortcut story URL (`_Shortcut: https://app.shortcut.com/summer/story/<number>_`) by reading the ticket number from the branch name. Leave it as-is.

## Step 3: Run the `gh pr create` command

Pass the body via heredoc to preserve newlines correctly:

```bash
gh pr create \
  --title "feature(idr): [sc-20032] Add new pay frequency question" \
  --body "$(cat <<'EOF'
_Shortcut:_

## What

Describe the problem this PR addresses.

## Why

Explain why this change is needed.

## How

What was changed and how it works.

## How to Test

Steps to verify the change works as expected.
EOF
)"
```

## Quick checklist before opening

- [ ] Branch name contains `sc-<number>/` or `ch-<number>/` (so the Shortcut link auto-populates)
- [ ] Title type and scope are from the allowed lists above
- [ ] Subject is 50 chars or fewer
- [ ] All four body sections are filled in (not just the headers)
- [ ] Use `sc-n/a` if there's no Shortcut ticket
