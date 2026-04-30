---
title: prefix+l — Jump to last layout
---

# prefix+l — Jump to the last layout you used

> **Language** · [中文](last-layout-zh.md) · **English** · [日本語](last-layout-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+l` jumps to the **most recently triggered** layout (not "the previous one in the list" — the previous one in your access history).

> Default chord: `l` · Config key: `last_layout` · Change in Settings → Hotkeys → Layout navigation

---

## How it differs from next/prev

| Action | Example behavior |
|---|---|
| `prefix+n` | List is [A, B, C], current is A: n → B; n → C; n → A |
| `prefix+l` | You visited A → B → A → C → A, current is A: l → C; l → A; l → C |

`prefix+l` is **toggle style**: it bounces between two frequently used layouts, no matter how many other layouts you passed through in between.

---

## How "the last one" is tracked

The engine maintains an **access history stack**:

- Whenever a new layout (different from the current one) fires successfully, the current layout is pushed onto the stack.
- `prefix+l` pops the most recent entry, treats it as the new current layout, and pushes the old current layout back on.
- Repeated `prefix+l` presses bounce back and forth between the most recent two layouts.

Events that record into history:

- `prefix+<key>` direct trigger.
- `prefix+w` selection confirmed with Enter.
- `prefix+n / p / l` themselves.

Events that **do not** record into history:

- `prefix+r` (restore — re-applying doesn't count as a switch).
- `prefix+w` cancellation (Esc / mouse / timeout).
- Failed / silently dropped triggers.

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| Nothing visited yet (first launch / only one layout ever triggered) | Warn, silent |
| The "last one" doesn't match the current display set (after hot-plug) | Warn, silent |
| The "last one" is locked under the free tier | Warn, silent |
