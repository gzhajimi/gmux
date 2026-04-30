---
title: prefix+z — Region temporary fullscreen
---

# prefix+z — Temporarily fullscreen the selected region

> **Language** · [中文](fullscreen-zh.md) · **English** · [日本語](fullscreen-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+z` enlarges the selected region's window to fill the **entire display's work area** (avoiding the taskbar). Press it again to restore.

> Default chord: `z` · Config key: `fullscreen` · Change in Settings → Hotkeys → Region actions

---

## Trigger flow

```
1. (optional) press prefix+q N    select a region; if you skip, region 0 is the default
2.            press prefix+z      the region's current window enlarges to fullscreen
3.        press prefix+z again    restore to the original region rectangle
```

---

## Why it's called "temporary fullscreen"

It **does not modify the variant**:

- The state lives only inside the current live layout.
- Switching to another binding and back → the fullscreen flag may persist (binding-scoped) or may become invalid because the regions changed.
- `prefix+r` (restore) clears every temporary fullscreen, returning the layout to the state described by TOML.
- Closing the region's current window or reloading the config also clears the matching fullscreen flag.

---

## Same-display single-fullscreen rule

To avoid the "two regions both fullscreened" visual conflict, the engine enforces **one fullscreen per display**:

> If region 1 on display A is already fullscreen and you `prefix+q 2` to select region 2 on display A and press `prefix+z`:
> - The engine first **demotes region 1** back to its original rectangle.
> - Then promotes region 2 to fullscreen.

Cross-display they don't interfere with each other. Region 1 on display A fullscreen + region 3 on display B fullscreen can coexist.

---

## Interaction with cycle_region (`prefix+f`)

If the selected region is currently fullscreen:

- The next window cycled in by `prefix+f` **inherits the fullscreen state** and is placed at fullscreen size.
- This lets you "fullscreen-rotate" through multiple windows of the same app.

---

## Interaction with close_window (`prefix+x`)

After `prefix+x` closes the region's window, the region's fullscreen flag is also cleared. The next window placed there won't be affected by the stale flag.

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| No layout currently available | Silent |
| selected_region doesn't exist or has no window in that slot | Silent; also clears the stale fullscreen flag for that region |
| The target window is dead | Silent |

---

## Not a real fullscreen API

"Fullscreen" here means "fill the work area" (avoiding the taskbar) via `maximize_native` (Win32 SW_MAXIMIZE). It is **not** the exclusive fullscreen of Win+Shift+Enter — it does not trigger a display-mode switch and it does not push games into the fullscreen rendering path.
