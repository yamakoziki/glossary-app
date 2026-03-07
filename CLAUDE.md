# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

多言語用語集アプリ (Multilingual Glossary App) — a fully static, no-build web application. The admin tool reads an Excel file, auto-translates terms via the Anthropic API, and generates a self-contained `docs/index.html` viewer for deployment on GitHub Pages.

## Development

No build step is needed. Open HTML files directly in a browser:

```bash
open admin/admin.html     # Admin/authoring tool
open docs/index.html      # Published glossary viewer
```

To inspect dependencies (only `xlsx` is used, and only via CDN in admin.html):

```bash
cat package.json
```

## Architecture

### Data Flow

1. **Source**: `data/glossary.xlsx` — Excel file with three sheets:
   - Terms sheet: Japanese terms + multilingual translations + image paths
   - Categories sheet: category master with translations
   - Settings sheet: app settings (supported languages, etc.)

2. **Admin tool** (`admin/admin.html`): A single-file, self-contained 5-step wizard:
   - Step 1: Load `data/glossary.xlsx` via SheetJS (from CDN)
   - Step 2: Review/edit terms; run AI translation via Anthropic API (`https://api.anthropic.com/v1/messages`) — API key stored in `localStorage`
   - Step 3: Validation
   - Step 4: Generate & preview HTML
   - Step 5: Download `index.html` → place in `docs/`

3. **Output**: `docs/index.html` — a fully self-contained viewer with all glossary data embedded as JSON. No server required. Images live in `docs/images/`.

### Excel Sheet Column Layout

**用語マスタ** (col index): id(0), catCode(1), nameJa(2), readingJa(3), summaryJa(4), detailJa(5), nameFr(6), summaryFr(7), detailFr(8), nameEn(9), summaryEn(10), detailEn(11), image(12), captionJa(13), captionFr(14), captionEn(15), published(16), name3(17), summary3(18), detail3(19), caption3(20)

**分類マスタ**: code(0), nameJa(1), reading(2), nameFr(3), nameEn(4), order(5), name3(6)

**設定**: key(0), value(1) — `サイトタイトル` sets the viewer page title.

`published` field: `1`, `true`, or `公開` = published; **empty string also defaults to published**.

### Key Implementation Notes

- All logic is vanilla JS inside `admin/admin.html` (~1474 lines). There is no separate JS/CSS file.
- Translation uses `claude-sonnet-4-20250514` via `callAnthropicApi()` (line ~498). Calls are sequential with a 1-second delay between requests for rate limiting.
- The generated `docs/index.html` embeds the viewer UI inline and all glossary data as JSON — produced by `buildIndexHtml()` (line ~939). Images are embedded as base64 `dataUrl` strings.
- `termImages` is an in-memory object (`{termIdx: [{filename, dataUrl}]}`) populated during Step 2. Images are not persisted across page reloads.
- SheetJS is loaded from `https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js` in admin.html. The `xlsx` npm package in `node_modules/` is not used at runtime.

### Deployment

GitHub Pages serves from `docs/` on the `main` branch. After generating a new `docs/index.html`:

```bash
git add docs/index.html docs/images/
git commit -m "update glossary"
git push
```
