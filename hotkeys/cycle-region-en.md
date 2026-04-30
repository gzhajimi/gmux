---
title: prefix+f — Cycle region windows
---

# prefix+f — Rotate through an app's multiple windows in a region

> **Language** · [中文](cycle-region-zh.md) · **English** · [日本語](cycle-region-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+f` brings the **next window** of the selected region's current app into that region.

> Default chord: `f` · Config key: `cycle_region` · Change in Settings → Hotkeys → Region actions

---

## Typical scenario

The layout puts Chrome in region 0, but you have 5 Chrome windows open. The first time the layout fires, it places the 1st Chrome window into region 0; to see the 2nd or 3rd window, press `prefix+f` repeatedly. Each press cycles to the next window and eventually wraps back to the start.

---

## Trigger flow

```
1. (optional) press prefix+q N    select region N; if you skip, region 0 is the default
2.            press prefix+f      the next window is moved into the region and activated
3.        press prefix+f again    the next-next window …
```

---

## How candidate windows are picked

The engine, in order:

1. Takes **all running windows** of the region's current app.
2. Excludes any window already used by **another region** in the current layout (so you don't steal another region's window).
3. Sorts the remainder by MRU (most recently used) into the cycle list.
4. On the first `f` press, picks the most recent entry that **isn't the current one**.
5. Subsequent `f` presses walk forward through the list, wrapping at the end.

> If there is only 1 candidate (including "the one already showing"), pressing `f` is **a silent no-op**.

---

## Cycle session lifetime

The interval between `f` presses is bounded by the **cycle timeout** (default `cycle_timeout_ms = 3000 ms`):

- Within 3 seconds, repeated `f` presses are treated as the same cycle session and the cursor keeps advancing.
- Press `f` more than 3 seconds later → start a fresh session, again from "not the current one".

The session is also **forcibly cleared** by these events:

- Switching layout (`prefix+<key>` / `prefix+n/p/l` / `prefix+w`).
- A window in the candidate list being closed.
- `prefix+r` (restore).
- `prefix+m` swap completion.
- Config reload.

---

## Fullscreen / native maximize handling

- If the current region is in `prefix+z` temporary fullscreen: the next window cycled in is also placed at **fullscreen size** (preserving the visual fullscreen).
- If the current layout marks the region with `is_native_maximize`: the cycled window goes through `maximize_native` instead of `move_window`, to avoid render artifacts in some apps (Office / browsers) at custom sizes.

---

## The selection sticks

`prefix+f` uses selected_region as its target. Once you select region N via `prefix+q N`, subsequent `f / z / g / c / x` presses all act on region N — **you don't have to re-select each time**.

selected_region only changes when you switch to another binding and back (each binding remembers its own).

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| No layout currently available | Silent |
| selected_region doesn't exist in the current layout | Silent |
| The region's app is not registered in `[apps]` | Error (shouldn't happen in theory) |
| Candidate list has fewer than 2 windows | Silent |
| The window the cursor points at is dead | Clear the session; this press is a no-op |

---

## Timing parameters

| Parameter | Default | Settings location |
|---|---|---|
| Max interval within one session | `3000 ms` | Hotkeys → Advanced → Cycle timeout (`cycle_timeout_ms`) |
