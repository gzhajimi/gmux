---
title: prefix+r — Re-apply current layout
---

# prefix+r — Re-run the current layout

> **Language** · [中文](restore-zh.md) · **English** · [日本語](restore-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+r` **re-runs the current layout exactly as described in TOML**: every region is re-placed, temporary fullscreens are cleared, and live splits are discarded.

> Default chord: `r` · Config key: `restore` · Change in Settings → Hotkeys → Layout navigation

---

## Most common scenarios

- You manually dragged some window and want it snapped back into place.
- You experimented with `prefix+c` split or `prefix+z` fullscreen and want the "clean" initial state back.
- Some app starts slowly and wasn't around when you first triggered the layout → after it finishes launching, `prefix+r` slots it into place per the variant.
- A multi-window app (e.g. four Chrome windows) ended up with the wrong instances placed; `prefix+r` lets MRU pick again.

---

## Trigger flow

Press `prefix+r` directly; no region selection or second key required.

---

## What it clears

- **selected_region**: the current binding's region selection is cleared, so the next `prefix+f / z / g / c / x` falls back to region 0.
- **Temporary fullscreen flags**: every region fullscreened via `prefix+z` returns to its original rectangle.
- **Live splits**: regions split out via `prefix+c` while live are discarded; the layout returns to the initial region set described in TOML.
- **Cycle session**: the `prefix+f` rotation cursor is cleared, and the next `f` press picks a fresh starting point.

---

## What it does **not** clear

- The config file itself doesn't change (restore doesn't write TOML).
- selected_region / fullscreen / split state of other bindings are untouched.
- Already-running app processes are untouched (no restart).
- Mouse / keyboard focus is untouched.

---

## Difference from re-triggering the same binding via `prefix+<key>`

If you pressed `prefix+1` to fire binding 1 and then press `prefix+1` again:

- Equivalent to `prefix+r`: re-runs the same layout.
- But the last_layout history is unchanged.

If you press `prefix+r`:

- Also re-runs the current layout.
- last_layout history is unchanged.

The practical difference is tiny; use whichever feels natural. The point of `prefix+r` is that it doesn't depend on you remembering which binding key is current.

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| No layout has ever been triggered (current_layout is empty) | Warn, silent |
| The current layout doesn't match the current display set (after hot-plug) | Warn, silent; pick a matching layout via `prefix+w` first |
