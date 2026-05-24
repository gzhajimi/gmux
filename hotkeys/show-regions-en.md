---
title: prefix+q — Select a region
---

# prefix+q — Show region numbers and select one

> **Language** · [中文](show-regions-zh.md) · **English** · [日本語](show-regions-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+q` is the prerequisite step for every "region action" (`prefix+f / z / g / c / x / d`): it paints the region numbers onto the screen, you press a digit to select one, and subsequent region actions act on that region. You can also, right after `prefix+q`, **press an action key then a digit** to run that action on a specific region within a single chord (see Pattern 2).

> Default chord: `q` · Config key: `show_regions` · Change in Settings → Hotkeys → Region operations

---

## What is a region

Each region of the current layout = **one slot of one display** (top-left / right half / middle band, etc.). When the OSD appears, every region's center shows its number in large digits, starting from 0.

> If no layout has been triggered yet, `prefix+q` is silently dropped — there's nothing to select.

---

## Four usage patterns

### Pattern 1: single-digit selection (most common)

```
press prefix
press q              ← every region on screen shows its number 0 1 2 3 …
press 3              ← selects region 3; the OSD flashes once and closes
```

After pressing the digit:

- The selected region **flashes-highlights** on the OSD as confirmation.
- The OSD closes.
- The selection is recorded as the current binding's selected_region and is used directly by:
  - `prefix+f` to cycle through the app's multiple windows in that region
  - `prefix+z` to temporarily fullscreen that region
  - `prefix+g` to jump focus to that region's window
  - `prefix+c` to add a new app into that region (split)
  - `prefix+x` to close that region's current window
  - `prefix+d` to reclaim that region (minimize its window, not close)

### Pattern 2: select-and-act in one chord (q + action + digit)

If you don't want the two-step "select first, act later" flow, then right after `prefix+q` **press the action key first, then the digit** — it selects that region and runs the action immediately, all in one chord:

```
press prefix
press q              ← every region on screen shows its number
press z              ← choose the action to run (here z = fullscreen)
press 3              ← selects region 3 and fullscreens it immediately
```

- The valid action keys are the chords of the six region actions (using their **current bound keys**, so rebinding carries over):
  `g` focus · `f` cycle windows · `z` fullscreen · `d` reclaim · `c` split · `x` close.
- After it runs, the region **stays selected**, so a subsequent bare `prefix+f / z / g / c / x / d` keeps acting on it.
- For numbers ≥ 10, use the second `q` for multi-digit mode the same way: `prefix` → `q` → `z` → `q` → `1` `2` → `Enter` (or submit by pausing for the timeout).
- A digit immediately after `q` (rather than an action key) is still Pattern 1's "select only" — unchanged behavior.
- `Esc` cancels. If the action key you press is disabled (its hotkey was cleared in Settings), it doesn't count as an action key — pressing it cancels and passes through.

### Pattern 3: two-digit selection (region number ≥ 10)

If the current layout has more than 10 regions (multi-display oct categories can reach 16+), use a second q to enter "multi-digit input mode":

```
press prefix
press q              ← OSD shows numbers
press q              ← enter multi-digit accumulate mode (OSD doesn't change)
press 1
press 2              ← buffer accumulates to "12"
press Enter          ← submit, selects region 12
```

Multi-digit mode details:

- Each digit press **resets** the input timeout (default `region_input_timeout_ms = 1000 ms`).
- After timeout, **Enter is auto-pressed**: the accumulated content is submitted as the number (e.g. buffer is `1`, submits region 1; empty buffer is equivalent to cancel).
- Press `Esc` to cancel; press `Enter` to submit immediately; pressing any other key cancels and passes that key through.

> If you only have ≤ 10 regions, you never need multi-digit mode — single digits suffice.

### Pattern 4: cancel

Ways to cancel after `prefix+q`:

- Press `Esc`: cancel; OSD closes.
- Press any key that isn't a digit, q, or Esc: cancel; OSD closes; **the key is passed through to the focused window**.
- Wait the overall timeout (default `cycle_timeout_ms = 3000 ms`): timeout cancel.
- Foreground window switch, display hot-plug, config reload: auto-cancel.

---

## How long the selection sticks

- **Persists across chords**: even hours later, `prefix+f / z / g / c / x / d` still acts on that region.
- **Per-binding**: each binding has its own selected_region. Switch to another binding and back, and the original selection is preserved.
- **Cleared by these events**:
  - After a config reload (`reload`), every selection is cleared.
  - When the region's window is closed, the engine automatically `forget`s the selection (the next region action falls back to region 0).
  - `prefix+r` (restore) clears the current binding's selection.

---

## What if I never selected a region

`prefix+f / z / g / c / x / d` automatically use **region 0** as the target when there is no selected_region. So the most common scenarios are:

- Single-region or single-fullscreen layouts: just `prefix+f` to cycle, `prefix+z` to fullscreen — no `prefix+q` needed.
- Multi-region layouts: `prefix+q N` once to set it, then keep using `prefix+f` / `prefix+z` without re-selecting each time.

---

## What if I pick an invalid number

If you press a digit that exceeds the current layout's region count, or the matching slot isn't a leaf (live layouts can dynamically merge):

- The OSD closes.
- selected_region is not modified (the previous selection is preserved).
- Subsequent region actions still operate on the prior selection.

---

## Timing parameters

| Parameter | Default | Settings location |
|---|---|---|
| Single-q mode total timeout | `3000 ms` | Hotkeys → Advanced → Cycle timeout (`cycle_timeout_ms`) |
| Multi-digit mode per-digit interval timeout | `1000 ms` | Hotkeys → Advanced → REGION number input timeout (`region_input_timeout_ms`) |
| OSD internal cleanup tick | `1000 ms` | Hotkeys → Advanced → REGION OSD cleanup interval |
