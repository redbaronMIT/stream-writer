# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

No build, lint, package manager, or test runner. The app is one static file.

- Run: `open index.html` (or double-click it). Reload the tab after each edit.
- Syntax-check the inline script after JS edits:
  ```
  python3 -c "import re;s=open('index.html').read();open('/tmp/check.js','w').write(re.search(r'<script>(.*)</script>',s,re.S).group(1))" && node --check /tmp/check.js
  ```
  Use the session scratchpad instead of `/tmp` if one is provided.
- There are no automated tests. Layout and scrolling behavior must be checked in a browser.

## Architecture

Everything lives in `index.html`: CSS in `<style>`, markup, and one IIFE in `<script>`. No dependencies. Backgrounds are hotlinked from `cdn.esahubble.org` and `images.unsplash.com`.

**Layout.** `main` (flex, centered) > `#frame` (optional box: padding, fill, radius) > `textarea#strip`. The textarea has no padding and is exactly `--lines` line-heights tall. Text can't be cut at the top or bottom edge because the scroll position is snapped to line boundaries by `snapToLines`: `"up"` (ceil) on input/keyup/click/settings change, `"round"` on scroll and resize. Doc view (`body.doc`) drops the fixed height and lets the textarea scroll normally, and `snapToLines` returns early there.

**Settings as CSS variables.** `applySettings()` writes `--lines`, `--ink`, `--fade`, `--box-bg`, `--pad-x`, `--pad-y` onto `<html>`. The stylesheet reads them, so CSS defaults in `:root` are only fallbacks. Settings persist under `stream-writer:settings:v1`. `--box-bg` is a vertical `linear-gradient` built by `boxBackground()` from box color, opacity, and box gradient. Box-only controls are disabled when the box is off.

**Persistence.** Text autosaves to `localStorage["stream-writer:v1"]`, debounced 250ms, and flushes on `pagehide`, `beforeunload`, and `visibilitychange: hidden`. "new" appends the current text to `stream-writer:v1:history` before clearing. All storage access is wrapped in try/catch because it can throw in private windows.

**Chrome hiding.** `body.typing` is added on input and removed after 1.5s or on mousemove. It hides the footer. Settings open via `#settings.open`.

**Background.** Two stacked `.bg` layers crossfade via the `.on` class. `setBackground` alternates between them.

## Conventions

- Keep it one file unless the user asks otherwise.
- Settings controls and their labels are plain `<label>` rows inside `#settings`. Adding a setting means updating `DEFAULTS`, `ui`, `applySettings`, `onSettingsInput`, and the markup.
- Settings labels are user-facing wording. Check with the user before renaming them.
