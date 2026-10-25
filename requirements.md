# Stream Writer: Requirements

## Purpose

A distraction-free tool for stream-of-consciousness writing. The writer types into a narrow strip with no visible record of earlier text, and never loses work. A full-document view exists, but it is secondary: the point is writing without looking back.

## Core behavior

1. **Full-screen inspirational background.** Real photographs: NASA/ESA space imagery (galaxies), trees, mountains, and similar landscapes. The background changes on request with a crossfade.
2. **Narrow vertical writing strip.** Centered horizontally and vertically on the page. It shows exactly N full lines of text (default 4). Older text scrolls out of view.
3. **Never show a partial line.** No line is cut off at the top or bottom edge of the strip at any time, including while typing, scrolling, or moving the caret.
4. **No typo highlighting.** Spellcheck underlines and autocorrect are off in the writing strip.
5. **Autosave.** Text saves continuously, so using the browser back button, closing the window, or switching apps loses nothing.
6. **Hide chrome while writing.** Footer controls fade out while typing and return on mouse movement or after a pause.
7. **Document view.** A toggle shows the full text in a wider, scrollable column. This does not change the saved text.
8. **New page.** Clears the strip. The previous text is kept in history first, not discarded.

## Settings

All settings persist across reloads.

| Setting | Behavior | Default |
|---|---|---|
| Lines | Number of full lines visible in the strip (1–12) | 4 |
| Show box | Draws a box behind the text | Off |
| Box opacity | Opacity of the box fill (0–100%). Only active when the box is shown | 35% |
| Box gradient | Fades the box fill from full opacity at the top to transparent at the bottom (0–100%). Only active when the box is shown | 0% (flat) |
| Box color | Color of the box fill. Only active when the box is shown | Black |
| Text color | Color of the text | Near-white |
| Text fade | How far the top of the strip fades out (0–100%). 0 is no fade | 40% |

## Interaction

- Keyboard shortcuts when the text strip is not focused: `B` changes background, `D` toggles document view.
- `Esc` closes the settings panel if open, otherwise toggles document view.
- `Cmd/Ctrl+Shift+Backspace` starts a new page.

## Non-goals

- Accounts, sync between devices, or sharing.
- Rich text, formatting, or export. Plain text only.
- Any build step or framework for the prototype. It is one static HTML file.

## Decisions

- **Gradient (Q1):** Two separate controls, box fill gradient and text top fade. Both stay.
- **Background images (Q2):** Keep hotlinks for the prototype. Self-hosting and license checks come before any public release.
- **Storage (Q3):** Local only (`localStorage`). No sync.
