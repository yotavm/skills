---
name: idea
description: >
  Capture and save ideas to yotav's personal idea vault. Use this skill whenever
  the user shares an idea, says "idea:", types /idea, mentions something they
  want to build, try, or explore, or asks to log/save/record a thought or
  concept. Each idea gets its own markdown file with a creative code name, saved
  to yotav/ideas/. If the idea is vague or short, ask exactly 2 clarifying
  questions before saving. Always use this skill — never just chat about an idea
  without saving it.
---

# Idea Capture Skill

Your job is to capture yotav's ideas and save each one as a beautifully formatted markdown file.

## Save location

Always save to: `/sessions/cool-zen-clarke/mnt/yotav/ideas/`

This maps directly to `/Users/yotavmasa/yotav/ideas` on yotav's Mac. Create the directory if it doesn't exist.

## The two-question rule

Read the idea yotav shared. If it's **specific enough** (has a clear what + why or how), skip straight to saving.

If it's **vague or very short** (e.g. just a title, no context, no detail), ask **exactly 2 clarifying questions** — no more, no fewer. Pick the 2 most valuable questions from things like:
- What problem does this solve?
- Who is this for?
- How would it work technically?
- What makes this different from existing solutions?
- What's the first step to make it real?

Wait for yotav's answers, then proceed to save.

## Code name

Give every idea a fun, punchy project code name. Format: `🔥 PROJECT [WORD]` — pick a word that captures the spirit of the idea (e.g. TUNNEL RAT for a tunneling tool, GHOST for something invisible, FORGE for something that builds things). Make it memorable and a little dramatic.

## File naming

Use kebab-case based on the idea topic:
`idea-[short-topic-slug].md`

Example: `idea-ai-meeting-summarizer.md`

## File format

```markdown
# Idea: [Title]
**Code Name: [emoji] PROJECT [WORD]**

**Date:** YYYY-MM-DD

## Summary
[1-2 sentence summary of the core idea]

## The Problem
[What gap or pain point does this address?]

## How It Works
[Brief description of the approach or mechanics]

## Why It's Interesting
[What makes this worth building? Unique angle, market gap, personal value, etc.]

## Potential Next Steps
- [First concrete action]
- [Second concrete action]
- [Third concrete action]
```

If yotav didn't provide detail for a section, use your best judgment to fill it in based on the idea — don't leave sections empty, but keep it concise.

## After saving

- Confirm with: "Saved! [View idea](computer:///sessions/cool-zen-clarke/mnt/yotav/ideas/[filename].md) — **[Code Name]**"
- Keep it short. Don't summarize the whole file back to yotav.
- Then ask: "What's the next idea?"
