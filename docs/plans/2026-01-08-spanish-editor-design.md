# Spanish Lesson Markdown Editor - Design

A minimal, bespoke markdown editor for taking notes on Spanish lessons.

## Goals

- Improve ease of taking Spanish lesson notes
- Make adding accents frictionless (á, é, í, ó, ú, ü, ñ, ¿, ¡)
- Simple keyboard shortcuts for saving
- Live preview of markdown

## Non-Goals

- AI features
- Nested folder navigation
- Sync scrolling
- Auto-save
- Syntax highlighting in editor
- Image drag-and-drop

## Architecture

**Stack:**
- Backend: Python + Flask
- Frontend: Plain HTML/CSS/JS (no build step)
- Editor: Textarea with custom JavaScript
- Preview: `marked.js` for live markdown rendering

**Launch:**
```bash
uv run editor.py --folder lessons/
```

Opens browser to `http://localhost:8741`.

**File structure:**
```
editor.py          # Flask server (~100-150 lines)
templates/
  index.html       # Editor UI
static/
  app.js           # Accent picker, shortcuts, save logic
  style.css        # Minimal styling
```

## UI Layout

```
┌─────────────────────────────────────────────────┐
│ [+ New]  lessons/                               │
│ ┌─────────────┐ ┌─────────────────────────────┐ │
│ │ nouns.qmd   │ │                             │ │
│ │ verbs.qmd ● │ │  (editor textarea)          │ │
│ │ greetings   │ │                             │ │
│ │             │ ├─────────────────────────────┤ │
│ │             │ │  (live preview)             │ │
│ │             │ │                             │ │
│ └─────────────┘ └─────────────────────────────┘ │
└─────────────────────────────────────────────────┘
```

- Left: flat list of `.qmd` files in folder, sorted alphabetically
- `●` indicator for unsaved changes
- Right top: editor textarea
- Right bottom: live markdown preview

## Accent Picker

Mimics macOS press-and-hold behavior with vim-style ergonomics.

**Trigger:** Hold any accent-able key for 350ms.

**Opposite-hand selection:** If holding a key with your right hand, selection keys are on your left hand (and vice versa).

| Key held | Hand  | Selection keys | Options      |
|----------|-------|----------------|--------------|
| a        | left  | j, k           | á            |
| e        | left  | j, k           | é            |
| u        | right | f, d           | ú (f), ü (d) |
| i        | right | f, d           | í            |
| o        | right | f, d           | ó            |
| n        | right | f, d           | ñ            |
| ?        | right | f, d           | ¿            |
| !        | left  | j, k           | ¡            |

**Behavior:**
1. Hold `u` → after 350ms, popup appears showing `f:ú  d:ü`
2. Tap `f` → inserts `ú`, replacing the `u` you were holding
3. Release without selecting → keeps original character
4. Escape → dismisses picker, keeps original
5. Uppercase preserved: Hold `A` → shows `Á`

## Keyboard Shortcuts

| Shortcut       | Action                        |
|----------------|-------------------------------|
| Cmd+S          | Save file to disk             |
| Cmd+Z          | Undo                          |
| Cmd+Shift+Z    | Redo                          |
| Escape         | Dismiss accent picker         |

**Save behavior:**
- POST to backend, writes to file on disk
- Brief visual feedback ("Saved" flash or border pulse)
- No auto-save; explicit saves only

**Undo/redo:**
- Custom JavaScript undo stack
- Debounced: saves state after 500ms of no typing
- Max 50 states

## File Management

**File list:**
- Shows all `.qmd` files in the target folder
- Click to open (prompts to save if unsaved changes)

**New file:**
- [+ New] button opens filename prompt
- Creates file with scaffolded frontmatter
- Opens immediately in editor

**Scaffolded frontmatter:**
```yaml
---
title: "Verbs Ser Estar"
description: ""
date: 2026-01-08
categories: []
topics: []
---

```

- Title derived from filename: `verbs-ser-estar.qmd` → `"Verbs Ser Estar"`
- Date set to today
- Other fields left empty

## Live Preview

- Updates on keystroke (debounced ~100ms)
- Renders tables, code blocks, lists, headers via `marked.js`
- Frontmatter displayed as styled metadata box (not raw YAML)
- Clean typography, legible table styling
- No sync scrolling for v1

## Inspiration

- [koaning/draft](https://github.com/koaning/draft) - Simple Flask + textarea approach, pragmatic design
