---
title: prefix+n — Next layout
---

# prefix+n — Switch to the next layout

> **Language** · [中文](next-layout-zh.md) · **English** · [日本語](next-layout-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+n` walks one step forward through the list of layouts that match the current display set.

> Default chord: `n` · Config key: `next_layout` · Change in Settings → Hotkeys → Layout navigation

---

## What it cycles through

- Scope = every binding that matches the currently connected displays (i.e. the same set listed by the `prefix+w` OSD).
- Order = the drag-and-drop order in the left column of the Layouts page.
- Wraps from the tail back to the head.

> If the current layout isn't in that list (e.g. just after a display hot-plug), `prefix+n` starts from the first item in the list.

---

## Layouts that are excluded

The following bindings will **not** be visited by `prefix+n`:

- No variant matches any currently connected physical display.
- Bindings locked under the free tier (10th onward, or multi-display variants on free).

---

## Trigger flow

Press `prefix+n` directly; no second key required.

You can press it rapidly: `prefix+n n n n …` walks through every matching layout in turn. Each press advances one step and immediately applies that layout.

---

## Difference from last_layout (`prefix+l`)

| Action | Semantics |
|---|---|
| `prefix+n` | Monotonic cycle: A → B → C → A → B → …, always forward |
| `prefix+p` | Monotonic cycle: A → C → B → A → C → …, always backward |
| `prefix+l` | Toggle: jumps to "the layout you used last", A → B → A → B → … (by access history) |

The two systems don't conflict: `n/p` is good for "let me see what each layout looks like" browsing; `l` is good for bouncing between two frequently used layouts.

---

## Rejection conditions

| Situation | Behavior (toast text shown when silent toast is enabled) |
|---|---|
| No layout matches the current displays | Silent / shows `step_no_layout` |
