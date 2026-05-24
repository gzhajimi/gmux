---
title: prefix+o — Open window (app picker)
---

# prefix+o — Open a window from the app picker and place it on the target monitor

> **Language** · [中文](open-window-zh.md) · **English** · [日本語](open-window-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+o` opens the same app picker as [`prefix+c`](split-region-en.md), then places the chosen app's window on the **target monitor** (by default the last monitor; can be overridden per the `prefix+m+digit+o` sequence described below) at a configurable size, centered. Unlike `prefix+c`, it does **not** create or occupy a region — the placed window is not tracked by restore, cycle, focus, or swap operations.

> Default chord: `o` · Config key: `open_window` · Size setting: `[hotkey].open_window_percent` (50–100, default 75) · Change chord in Settings → Hotkeys → Region operations

---

## Trigger flow

```
1. press prefix+o          the app picker overlay appears
2. type to filter           narrow the app list by name
3. press Enter (or click)   the chosen app's window is placed on the target monitor
```

---

## Window placement

The window is placed on the **target display**, centered:

- **Default target**: the last display (highest display index in the current system geometry).
- **Override**: run `prefix+m` → display digit → `o` (see [prefix+m — Swap displays](swap-displays-en.md)) to pick a different display and set it as the sticky target. After that, `prefix+o` opens on the chosen display instead of the last one. The override is cleared on restart, display hot-plug / re-enumeration, or the next `prefix+m+digit+o` sequence.
- **Size**: controlled by `[hotkey].open_window_percent` (50–100). `100` = fill the entire work area of that monitor; `50` = half the work area.
- **Position**: centered on the target monitor's work area.
- No region is created. The window does not participate in the layout and is not remembered by binding/region slot memory.

---

## Reuse-then-spawn behavior

Before launching a new instance, the engine looks for a **free window** of the chosen app — one that is not already placed in the current layout:

| App window state | What happens |
|---|---|
| A free (unplaced) window exists | That window is moved to the last monitor and sized/centered |
| No free window; app is running | A new instance is launched, then placed |
| App is not running | App is launched, then placed |

The engine **never closes** an existing window to satisfy this action (non-destructive).

---

## What is NOT affected

`prefix+o` does **not** interact with layout-aware operations:

- `prefix+r` (restore) — does not restore or move the opened window
- `prefix+f` (cycle_region) — does not cycle through it
- `prefix+g` (focus_region) — does not target it
- `prefix+m` (swap_displays) — does not move it
- Region slot memory — the window is not tracked per region

Re-pressing `prefix+o` simply reopens the picker; the previously placed window is left as-is.

---

## Difference from prefix+c (split_region)

Both actions open the same app picker. The difference is what happens after you choose an app:

| | `prefix+o` open_window | `prefix+c` split_region |
|---|---|---|
| Creates a region | No | Yes (splits selected region) |
| Monitor | Target monitor (last by default; overridable via `prefix+m+digit+o`) | Selected region's monitor |
| Size | `open_window_percent` (50–100 %) | Determined by the split layout |
| Tracked by restore/cycle/focus | No | Yes |
| Affects layout | No | Yes |

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| Picker dismissed (Esc / no selection) | Silent; no window is moved or launched |
| App definition has no `exe` and is not running | Silent; picker closes normally |

---

## Configuration

Add to your `config.toml` to change the chord or placement size:

```toml
[hotkey]
open_window_percent = 80   # 50–100; default 75 (100 = fill work area)

[actions]
open_window = "o"          # default; set to "" to disable
```

Change in Settings → Hotkeys → Advanced → "Open size" slider.
