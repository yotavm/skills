---
name: slide-deck
description: Generate a polished HTML slide deck using the established design system (container-query layout, blue accent palette, prebuilt component classes). Use this skill whenever the user asks to "create slides", "make a slide deck", "build a presentation", "/slide-deck", needs slides for a workshop / talk / brown-bag / demo / PEDD / readout, or wants to turn a doc into slides. Trigger even when the user does not explicitly say "HTML" — this is the default presentation format. Always use this skill rather than improvising one-off CSS, so the aesthetic stays identical across decks.
---

# slide-deck

Generates a single self-contained HTML file that matches the visual system used in `claude-workshop/slides/index.html` and `sunscreen/junior-agent/plans/demo/slides.html`. Same fonts, same `--accent: #2179aa` blue, same 960×540 (16:9) cqi-based layout, same prebuilt components, same nav (single-slide / view-all / fullscreen, arrow keys, URL hash).

The point of this skill is **consistency**. Don't invent new visual styles, new color schemes, or new component classes. Compose new decks by reusing the building blocks already defined in the template. If something doesn't fit a built-in component, prefer recombining existing components over adding new CSS.

## Workflow

1. **Decide the output path.** Default to `./slides/index.html` relative to the user's current working dir, or whatever path they specify. Confirm if ambiguous.
2. **Copy the template** at `assets/template.html` (sibling of this SKILL.md) to the output path. The template already contains the full `<style>` block, the nav bar, the keyboard/fullscreen/hash JS, and an empty `<div class="slides-container" id="slides">` with a `<!-- SLIDES GO HERE -->` placeholder.
   ```bash
   cp <skill-dir>/assets/template.html <output-path>
   ```
3. **Replace the placeholder** with your slide `<div class="slide" data-slide="N">…</div>` blocks, in order. Use the component recipes below.
4. **Update the counter default** in the nav: change `<div class="counter" id="counter">1 / 20</div>` so the `20` matches the real total. (The JS recomputes `total` from `[data-slide]`, so this is just the pre-JS placeholder.)
5. **Number every slide** with `<span class="slide-number">N / TOTAL</span>` as the first child inside the `.slide` div, and set `data-slide="N"` to match.
6. **Open the file** so the user can see it (`open <path>` on macOS), unless they said not to.

## The design contract — what to keep identical

These pieces define the look. Don't change them per deck:

- **CSS variables** (defined in `:root`):
  - `--accent: #2179aa` (the blue) — accents, step-num pills, borders, eyebrow text
  - `--accent2: #4a9bcb` (lighter blue) — bot avatars, secondary accents
  - `--accent-soft: rgba(33,121,170,0.08)` — soft fills for highlight boxes / pill badges
  - `--bg: #ffffff`, `--surface: #f8fafc`, `--border: #e2e8f0`, `--text: #0f172a`, `--muted: #64748b`, `--code-bg: #f1f5f9`, `--green: #16a34a`, `--yellow: #b45309`
  - `--slide-w: 960px`, `--slide-h: 540px` (16:9)
- **Slide frame**: every content slide is `<div class="slide" data-slide="N">`. The frame uses `container-type: inline-size`, so all sizing inside should use `cqi` units — never `px`/`rem`/`em` for slide-internal text or spacing. `cqi` = 1% of the slide width, so the deck scales identically in single-slide, view-all, and fullscreen modes.
- **Page font**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif` — already on `body`. Don't add custom fonts.
- **Nav bar + JS**: keep as-is. Hash routing, arrow keys, F for fullscreen, A for view-all all work out of the box.

## Slide types

### Title slide (slide 1)
Use `class="slide slide-title"`. Structure: `eyebrow → h1 → subtitle → meta`. The h1 has a subtle gradient text fill — keep that styling. If the title is long, break with `<br>` and use the half-size span pattern from the workshop deck.

### Section header (optional, between groups of slides)
Use `class="slide slide-section"`. Big h2, small `.section-meta` mono caption. Best for marking "Part 2 — Skills" style transitions.

### Content slides (most common)
Use `class="slide"`. First inner element after the slide-number is usually `<div class="slide-label"><span class="step-num">N</span>Section name</div>`, then `<h2>Slide title</h2>`, then the body. Use `.two-col` for split layout, or just block content for single-column.

### Demo / "live" callout slide
Use `class="slide slide-demo"`. Centered, dashed border, includes `.demo-badge` with pulsing dot, an h2, and a row of `.demo-step-pill` elements.

### Wrap-up slide (last slide, "Questions?", thanks, take-homes)
Use `class="slide slide-wrap"`. Big centered h2, `.sub` paragraph, optional `.q` line for "Questions?".

## Component cheat-sheet

Use these classes — they're already styled. Reach for them in this rough order:

| Need to show… | Use |
|---|---|
| Bulleted feature list with arrows | `.feature-list` with `.icon → .label .desc` per item |
| Problems / pain points | `.problem-list` (left-bordered cards with `.ptitle` / `.pdesc`) |
| Numbered agenda or flow | `.flow-list` (numbered circles, optional `.duration` pill) |
| Big lead paragraph | `.lede` (with `.accent` spans for emphasis), optional `.lede-sub` below |
| Small callout boxes in a column | `.arch-callouts` containing `.arch-callout` (add `.accent` for emphasized) |
| Highlight banner / soft blue card | `.highlight-box` |
| Pills with eyebrow + text | `.pill-grid` of `.pill` items, each with optional `.when` eyebrow |
| Good vs bad comparison | `.compare` with `.compare-card.bad` / `.compare-card.good` |
| Three-up summary cards | `.combo-grid` of `.combo-card` (role / what / why) |
| Recipe-style "skill card" | `.skill-card` (header + trigger section + recipe section) |
| Step-by-step architecture flow | `.arch-flow` of `.arch-step` (numbered dot + title + detail) |
| Two-up cards (e.g. agents) | `.agent-grid` of `.agent-card` |
| Schedule / cron settings mockup | `.schedule-modal` |
| Claude Desktop chat mockup | `.claude-mockup` (titlebar with traffic-light dots, msg avatars) |
| Slack chat mockup | `.slack-mockup` |
| Connectors list mockup | `.connectors-mockup` (header + `.connectors-list`) |
| Status badges (Next / Done / ?) | `.status-badge.badge-next / .badge-done / .badge-q` |
| Numbered walkthrough steps | `.ex-steps` (large numbered circles, step title + `.desc`) |
| Two-up exercise cards | `.ex-overview` of `.ex-card` |
| Build-with-me paste blocks | `.build-grid` of `.build-col` containing `.paste-box` |

If you need a layout the cheat-sheet doesn't cover, **first** try composing existing components inside `.two-col` or a CSS grid with inline `style="grid-template-columns: …; gap: …cqi;"`. Don't add new CSS to `<style>` — it makes future decks drift.

## Reference snippets

These are copy-paste-ready. Tweak the text, keep the structure.

### Example 1 — Title slide

```html
<div class="slide slide-title" data-slide="1">
  <span class="slide-number">1 / 12</span>
  <div class="eyebrow">Team Readout · 30 min</div>
  <h1>Shipping Faster<br><span style="font-size: 0.55em; -webkit-text-fill-color: var(--accent); background: none; font-weight: 600;">with AI agents in the loop</span></h1>
  <div class="subtitle">What we built, what worked, what's next.</div>
  <div class="meta">Q2 2026 · Engineering</div>
</div>
```

### Example 2 — Two-column content slide with feature-list + arch-callouts

```html
<div class="slide" data-slide="2">
  <span class="slide-number">2 / 12</span>
  <div class="slide-label"><span class="step-num">1</span>Context</div>
  <h2>What we set out to fix</h2>
  <div class="two-col" style="grid-template-columns: 1.05fr 1fr;">
    <div>
      <p class="lede">Engineers were spending hours on <span class="accent">small fixes</span> that should take minutes.</p>
      <ul class="feature-list" style="margin-top: 1.6cqi;">
        <li><span class="icon">→</span><span><span class="label">Context-switching cost.</span><span class="desc"> Every small ticket pulled focus from real work.</span></span></li>
        <li><span class="icon">→</span><span><span class="label">Backlog drift.</span><span class="desc"> Low-priority bugs piled up sprint after sprint.</span></span></li>
        <li><span class="icon">→</span><span><span class="label">Repeated questions.</span><span class="desc"> Same Slack threads, same answers, every week.</span></span></li>
      </ul>
    </div>
    <div>
      <div class="arch-callouts">
        <div class="arch-callout accent"><strong>The bet</strong><br>An agent that handles small tickets end-to-end could buy back a full day per engineer per sprint.</div>
        <div class="arch-callout"><strong>Constraint</strong><br>Has to plug into existing tools — no new dashboards, no new processes.</div>
        <div class="arch-callout"><strong>Success</strong><br>PRs we'd merge without changes.</div>
      </div>
    </div>
  </div>
</div>
```

### Example 3 — Numbered agenda / flow slide

```html
<div class="slide" data-slide="3">
  <span class="slide-number">3 / 12</span>
  <div class="slide-label"><span class="step-num">1</span>Context</div>
  <h2>What we'll cover</h2>
  <ol class="flow-list">
    <li><div class="num">1</div><div class="title">The problem we picked &amp; why</div><div class="duration">5 min</div></li>
    <li><div class="num">2</div><div class="title">The agent: how it actually works</div><div class="duration">10 min</div></li>
    <li><div class="num">3</div><div class="title">Live demo — label a ticket, watch it ship</div><div class="duration">8 min</div></li>
    <li><div class="num">4</div><div class="title">Results from real PRs</div><div class="duration">5 min</div></li>
    <li><div class="num">5</div><div class="title">What's next + Q&amp;A</div><div class="duration">2 min</div></li>
  </ol>
</div>
```

### Example 4 — Wrap slide

```html
<div class="slide slide-wrap" data-slide="12">
  <span class="slide-number">12 / 12</span>
  <h2>That's the gist.</h2>
  <div class="sub">
    The pattern works for any "we do this every sprint" workflow.<br>
    Pick one and we'll prototype it together.
  </div>
  <div class="q">Questions?</div>
</div>
```

## Common mistakes to avoid

- **Don't use `px` for text sizes inside slides.** Use `cqi`. Body-level chrome (the nav bar, body padding) uses px — that's intentional, it's outside the container query context.
- **Don't redefine `--accent`** to a different color per deck. The shared blue is the brand. If the user explicitly asks for a different palette, change `--accent` and `--accent2` in `:root` and also update the title-slide `::before` radial-gradient rgba values to match.
- **Don't drop the `<span class="slide-number">` on a slide.** It's the orientation cue.
- **Don't forget `data-slide="N"` on every slide.** The nav script counts `[data-slide]`; missing the attribute means the slide is invisible.
- **Don't add `<style>` overrides for one-off slides.** Use inline `style=""` on the specific element instead. Keeps the global CSS clean and decks consistent.
- **Don't introduce icon libraries or images.** The decks use Unicode arrows (→), pulse dots, traffic-light circles, and inline SVGs only. Stick to that.

## When the user gives you content

If they hand you a doc, an outline, or bullet points, your job is to **map their content onto the component types above**, not to design from scratch. Read their input, decide which components fit each section, draft the slides, then ask if they want changes. Slide count: aim for ~6–15 for a 20–40 min talk. Don't pad with redundant slides.

If their content is sparse, ask one clarifying question: *"Roughly how long is this talk and who's the audience?"* — that anchors slide count and tone. Don't ask more than that.
