---
name: publish-talk
description: Publish a presentation from the sibling talk-builder repo to public/slides for hosting on itsaydrian.dev. Use when the user mentions publishing, copying, or deploying slides, presentations, or talks from talk-builder to this site. Also use when the user wants to link slides on the speaking page or asks about making a talk's slides available online.
---

# Publish Talk

Copies a self-contained HTML presentation from the `talk-builder` repo (a sibling directory at `../talk-builder`) into `public/slides/` so it's served on itsaydrian.dev, then links it from the speaking page.

## Source and destination

- **Source:** `../talk-builder/talks/{slug}/` (relative to project root)
- **Destination:** `public/slides/{slug}/`

Each talk in talk-builder is identified by its directory name (the "slug"), e.g. `trust-but-verify`, `i-agent`.

## Workflow

### 1. Identify the talk

If the user didn't specify which talk, list the directories in `../talk-builder/talks/` and ask which one to publish. Also check `public/slides/` to see which talks are already published — flag if the talk already exists (the user may want to update it).

### 2. Copy presentation files

Copy **only** these from the talk directory:

- `index.html` — the self-contained presentation (inline CSS/JS, no external deps)
- `assets/` — images, SVGs, and the og-card used by the presentation

**Do not copy** authoring artifacts — these stay in talk-builder:
- Markdown files: `TALK.md`, `SCRIPT.md`, `DEMO.md`, `OUTLINE.md`, `SCRATCH_PAD.md`
- Support directories: `demo-docs/`, `scripts/`

Create the destination directory structure first:
```
public/slides/{slug}/
public/slides/{slug}/assets/
```

### 3. Fix og:image meta tag

The talk-builder generates `index.html` with a relative og:image path (`assets/og-card.png`). Social-sharing crawlers need an absolute path to resolve the image correctly.

In the copied `index.html`, find:
```html
<meta property="og:image" content="assets/og-card.png">
```

Replace with:
```html
<meta property="og:image" content="/slides/{slug}/assets/og-card.png">
```

Use the actual slug in the path. If the og:image content already starts with `/`, it's already absolute — skip this step.

### 4. Update speaking.astro

Open `src/pages/speaking.astro`. The `talks` array contains entries for each talk appearance. Find entries whose title matches the talk being published and whose `slides` property is `null`.

Read the talk's `TALK.md` from talk-builder (`../talk-builder/talks/{slug}/TALK.md`) to get the exact title for matching.

Set the `slides` property to `'/slides/{slug}/'` for all matching entries. A single talk may appear at multiple events — update all of them.

**Example:** For slug `i-agent` with title "I, Agent: Identity-First Security for the Autonomous Future":
```javascript
// Before
slides: null,

// After
slides: '/slides/i-agent/',
```

### 5. Verify

Run `bun run build` and confirm no errors. The slides are static files in `public/` so they don't go through Astro's build pipeline, but this catches any issues with the speaking.astro changes.
