---
name: pr-screenshots
description: "Capture, annotate, and include screenshots in pull requests for UI changes. Use when creating or updating PRs that touch frontend components, pages, or any web-facing surface. Also use when asked to add before/after screenshots, visual diffs, or enrich PR descriptions. Triggers on: PR screenshots, before/after, visual diff, PR description, capture screenshot, PR images, enrich PR."
license: MIT
metadata:
  author: "inkeep"
  version: "1.0"
---

# PR Screenshots

Capture, redact, annotate, and embed screenshots in GitHub PRs for UI changes.

## When to use

- Creating/updating PRs that touch frontend components, pages, or styles
- User asks for screenshots, before/after comparisons, or PR body enrichment
- Skip for backend-only, test-only, or non-visual changes

## Prerequisites

These scripts require the following npm packages. Install them as dev dependencies in your project:

| Package | Purpose | Install |
|---|---|---|
| `playwright` | Browser automation for screenshot capture | `npm add -D playwright` |
| `sharp` | Image annotation (labels, borders, stitching) | `npm add -D sharp` |
| `tsx` | TypeScript runner for scripts | `npm add -D tsx` |

After installing Playwright, download browser binaries: `npx playwright install chromium`

## Workflow

1. **Identify affected pages** from the PR diff
2. **Capture screenshots** — run `scripts/capture.ts`
3. **Validate no sensitive data** — run `scripts/validate-sensitive.ts`
4. **Annotate** — run `scripts/annotate.ts` (labels, borders, side-by-side)
5. **Upload & embed** — update PR body with images

---

## Step 1: Identify Affected Pages

Analyze the PR diff to determine which UI routes are impacted. Map changed component/page files to their corresponding URLs. If the diff only touches backend code, tests, or non-visual files, skip screenshot capture.

---

## Step 2: Capture Screenshots

### Environment setup

| Environment | Base URL | Notes |
|---|---|---|
| **Local dev** | `http://localhost:3000` (or your dev server port) | Start your dev server first |
| **Preview deployment** | Your preview URL (e.g., Vercel, Netlify, etc.) | Available after PR push |
| **Playwright server** | Connect via `--connect ws://localhost:3001` | See "Reusable server" below |

### Capture command

```bash
# Local dev
npx tsx scripts/capture.ts \
  --base-url http://localhost:3000 \
  --routes "/dashboard,/settings" \
  --output-dir ./pr-screenshots

# Preview deployment
npx tsx scripts/capture.ts \
  --base-url https://your-preview-url.example.com \
  --routes "/dashboard,/settings" \
  --output-dir ./pr-screenshots

# With Playwright server (reuses browser across captures)
npx tsx scripts/capture.ts \
  --connect ws://localhost:3001 \
  --base-url http://localhost:3000 \
  --routes "/dashboard,/settings" \
  --output-dir ./pr-screenshots
```

### All capture options

| Option | Default | Description |
|---|---|---|
| `--base-url <url>` | *required* | Target URL (local dev or preview) |
| `--routes <paths>` | *required* | Comma-separated route paths |
| `--output-dir <dir>` | `./pr-screenshots` | Where to save PNGs and DOM text |
| `--viewport <WxH>` | `1280x800` | Browser viewport size |
| `--connect <ws-url>` | — | Connect to existing Playwright server |
| `--mask-selectors <s>` | — | Additional CSS selectors to blur |
| `--wait <ms>` | `2000` | Wait after page load before capture |
| `--full-page` | `false` | Capture full scrollable page |
| `--auth-cookie <value>` | — | Session cookie for authenticated pages |

### Reusable Playwright server

Start a server once, reuse across multiple captures:

```bash
# Terminal 1: start server
npx tsx scripts/capture.ts --serve --port 3001

# Terminal 2+: connect and capture
npx tsx scripts/capture.ts \
  --connect ws://localhost:3001 --base-url http://localhost:3000 \
  --routes "/..." --output-dir ./pr-screenshots
```

---

## Step 3: Validate Sensitive Data

**Always run before uploading to GitHub.**

```bash
npx tsx scripts/validate-sensitive.ts \
  --dir ./pr-screenshots
```

The script checks `.dom-text.txt` files (saved by capture) for:
- API keys (`sk-`, `sk-ant-`, `AKIA`, `sk_live_`)
- Tokens (Bearer, JWT, GitHub PATs)
- PEM private keys
- Connection strings with credentials

Exit code 1 = sensitive data found. Re-capture with additional `--mask-selectors` or fix the source before proceeding.

### Pre-capture masking (automatic)

The capture script automatically masks these before taking screenshots:

| Selector / Pattern | What it catches |
|---|---|
| `input[type="password"]` | Password fields |
| Text matching `sk-`, `Bearer`, `eyJ`, `ghp_`, PEM headers | In-page tokens/keys |

Add more with `--mask-selectors "selector1,selector2"`.

---

## Step 4: Annotate Images

```bash
# Add "Before" label with red border
npx tsx scripts/annotate.ts \
  --input before.png --label "Before" --border "#ef4444" --output before-labeled.png

# Add "After" label with green border
npx tsx scripts/annotate.ts \
  --input after.png --label "After" --border "#22c55e" --output after-labeled.png

# Side-by-side comparison
npx tsx scripts/annotate.ts \
  --stitch before.png after.png --labels "Before,After" --output comparison.png
```

---

## Step 5: Upload & Embed in PR

### Upload images to GitHub

Images in PR markdown need permanent URLs. Use one of:

**Option A — PR comment with image** (simplest):
```bash
# GitHub renders attached images with permanent CDN URLs
gh pr comment {pr-number} --body "![Before](./pr-screenshots/before-labeled.png)"
```

**Option B — Update PR body directly**:
```bash
gh pr edit {pr-number} --body "$(cat pr-body.md)"
```

### PR body templates

Use the templates in [references/pr-templates.md](references/pr-templates.md) for consistent formatting. Include:

1. **Visual Changes** section with before/after screenshots
2. **Test URLs** section with links to preview deployment pages
3. **Summary** of what changed and why

---

## Additional Resources

- [references/pr-templates.md](references/pr-templates.md) — PR body markdown templates
