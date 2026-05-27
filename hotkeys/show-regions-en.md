---
title: prefix+q — Select a region
---

# prefix+q — Show region numbers and select one

> **Language** · [中文](show-regions-zh.md) · **English** · [日本語](show-regions-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+q` is the prerequisite step for every "region action" (`prefix+f / z / g / c / x / d`): it paints the region numbers onto the screen. From there you can either **just select a region** (type the number, then `Enter`/pause), or **select-and-act in one chord** — in either order: **number first** (`q→digit→action key`, recommended) or **action first** (`q→action key→digit`, the old order, retained).

> Default chord: `q` · Config key: `show_regions` · Change in Settings → Hotkeys → Region operations

---

## What is a region

Each region of the current layout = **one slot of one display** (top-left / right half / middle band, etc.). When the OSD appears, every region's center shows its number in large digits, starting from 0.

> If no layout has been triggered yet, `prefix+q` is silently dropped — there's nothing to select.

---

## Four usage patterns

### Pattern 1: select a region (number first, select only)

```
press prefix
press q              ← every region on screen shows its number 0 1 2 3 …
press 3              ← type the number (multi-digit OK, e.g. 1 2 = 12)
press Enter          ← lock in the selection (or just pause ~1 s for the input timeout)
```

Notes:

- **Difference from the old behavior**: pressing the number digit **no longer takes effect instantly**. Press `Enter`, or pause for the input timeout (`input_timeout_ms`, default 3000 ms), to lock it in. This is what lets an action key follow the number (see Pattern 2).
- Multi-digit numbers are **typed straight through** — no second `q` needed; each digit resets the input timeout.
- Once locked in, the selection is recorded as the current binding's selected_region and is used directly by:
  - `prefix+f` to cycle through the app's multiple windows in that region
  - `prefix+z` to temporarily fullscreen that region
  - `prefix+g` to jump focus to that region's window
  - `prefix+c` to add a new app into that region (split)
  - `prefix+x` to close that region's current window
  - `prefix+d` to reclaim that region (minimize its window, not close)
- An empty buffer on `Enter`/timeout = cancel (nothing selected).

### Pattern 2: select-and-act, number first (recommended)

If you don't want the two-step "select first, act later" flow, **type the number first, then press the action key** — it selects that region and runs the action in one chord:

```
press prefix
press q              ← every region on screen shows its number
press 3              ← type the number (multi-digit OK)
press z              ← action key; runs the action on region 3 immediately (here z = fullscreen)
```

- The action key also **terminates the number input** — pressing it submits, no `Enter` needed.
- The valid action keys are the chords of the six region actions (using their **current bound keys**, so rebinding carries over):
  `g` focus · `f` cycle windows · `z` fullscreen · `d` reclaim · `c` split · `x` close.
- After it runs, the region **stays selected**, so a subsequent bare `prefix+f / z / g / c / x / d` keeps acting on it.
- Multi-digit works the same way: `q → 1 → 2 → z` fullscreens region 12.

### Pattern 3: select-and-act, action first (old order, retained)

You can still use the old order: **press the action key first, then the number**.

```
press prefix
press q              ← every region on screen shows its number
press z              ← choose the action to run first (here z = fullscreen)
press 3              ← then the digit; selects region 3 and fullscreens it immediately
```

- With the action key first, **a single digit fires instantly** (no `Enter`).
- For numbers ≥ 10: after the action key, press a second `q` to enter the multi-digit accumulator, then submit with `Enter` or by pausing for the timeout — `prefix` → `q` → `z` → `q` → `1` `2` → `Enter`.
- `Esc` cancels. If the action key you press is disabled (its hotkey was cleared in Settings), it doesn't count as an action key — pressing it cancels and passes through.

### Pattern 4: cancel

Ways to cancel after `prefix+q`:

- Press `Esc`: cancel; OSD closes.
- Press a key that isn't a digit, an action key, `q`, `Enter`, or `Esc`: cancel; OSD closes; **the key is passed through to the focused window**.
- Wait for a timeout — two cases:
  - **No number typed yet** (right after `prefix+q`, or action-first before the digit): the input timeout (`input_timeout_ms`, default 3000 ms) cancels.
  - **A number already typed**: the input timeout (`input_timeout_ms`, default 3000 ms) applies, and timing out **submits the typed number as the selection** (see Pattern 1) — it does **not** cancel.
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
- Multi-region layouts: use `prefix+q` once to select a region (type the number, then `Enter`/pause to lock it in), then keep using `prefix+f` / `prefix+z` without re-selecting each time.

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
| Timeout before any number is typed | `3000 ms` | Hotkeys → Advanced → Post-command input timeout (`input_timeout_ms`) |
| Number input timeout (reset on each digit; pausing locks in the selection) | `3000 ms` | Hotkeys → Advanced → Post-command input timeout (`input_timeout_ms`) |
| OSD internal cleanup tick | `1000 ms` | Hotkeys → Advanced → REGION OSD cleanup interval |
