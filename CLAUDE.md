# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

darktable user manual — Markdown source rendered to HTML (and PDF/ePub) via [Hugo](https://gohugo.io) (extended). The primary language is English; translations live in `po/` as `.po` files managed through Weblate, not edited directly in git.

## Local dev server

```bash
hugo server -D --disableFastRender
# available at http://localhost:1313/usermanual/development/
```

Requires Hugo *extended* and that Bootstrap assets were built once:

```bash
cd themes/hugo-darktable-docs-theme/assets/ && npm install
```

## Build commands

| Task | Command |
|------|---------|
| Production HTML | `hugo` (output in `public/`) |
| HTML (script, with disabled-language support) | `tools/build-html.sh [base_url]` |
| PDF | `tools/build-pdf.sh` (needs `weasyprint`) |
| ePub | `tools/build-epub.sh` |
| Check broken links | CI uses `lychee` against a running `hugo server` |
| Generate/update translations | `tools/generate-translations.sh --no-update` (needs `po4a` ≥ 0.58) |
| Remove generated translation files | `tools/generate-translations.sh --rm-translations` |

Disabled languages are listed in `disable-languages` (space-separated codes) and excluded from builds automatically.

## Content structure

```
content/
  <section>/
    _index.md          # metadata only (title, id, weight) — no body text
    <subsection>/
      _index.md
      page.md
      page/            # media for page.md lives here
        image.png
```

Key content sections: `overview`, `lighttable`, `darkroom`, `module-reference`, `preferences-settings`, `special-topics`, `guides-tutorials`.

## Writing conventions (enforce these on edits)

- **Minimalism**: fewer words, shorter files.
- **Case**: all headers/titles lowercase except top-level chapter names; match darktable GUI exactly.
- **Headers**: never exceed `###` (level 3).
- **No shortcodes, no raw HTML** (keep markdown portable).
- **No column wrapping**; files must be UTF-8.
- `_index.md` files contain metadata only — no prose content.
- Images default to 100 % width; use `#w25` / `#w33` / `#w50` / `#w66` / `#w75` suffixes to resize, `#icon` for inline icons, `#inline` to make block images inline.
- Screenshots must use the default **darktable-elegant-grey** theme.
- Module controls documented as **definition lists**, not bullet lists.
- Processing-module links in *italics*; utility-module links in plain text.
- Internal links must be relative (start with `./` or `../`) and point to `.md` files.
- Notes formatted as: `---` / `**Note**: text.` / `---`
- Keyboard key names in CamelCase (`Ctrl`, `Shift`); single letters lowercase (`"h"`); key combos joined with `+` (`Ctrl+Shift+h`); mouse actions lowercase-hyphenated (`right-click`, `double-click`).

## Page metadata (frontmatter)

```yaml
---
title: page title
id: page-id        # matches filename (or parent dir for _index.md)
weight: 10         # controls ToC order; omit for alphabetical
---
```

## Translations

- All translation work goes through [Weblate](https://weblate.pixls.us/projects/darktable/) — **do not commit `.po` file edits directly**.
- Generated translated `.md` files are never checked in; always remove them with `--rm-translations` before committing.
- Theme i18n strings live in `themes/hugo-darktable-docs-theme/i18n/<lang>.yaml`.

## PR expectations

- PRs must link to the darktable source PR being documented (label: `documentation-pending`).
- CI checks: broken links (`lychee`), translation generation, and full `hugo` build — all run under Nix (`nix develop`).
