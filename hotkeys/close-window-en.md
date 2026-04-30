---
title: prefix+x — Close region window
---

# prefix+x — Close the current window in the selected region

> **Language** · [中文](close-window-zh.md) · **English** · [日本語](close-window-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+x` closes the one window currently in the selected region. It **doesn't restart the app**, it only closes that one window.

> Default chord: `x` · Config key: `close_window` · Change in Settings → Hotkeys → Region actions

---

## Trigger flow

```
1. (optional) press prefix+q N    select a region; if you skip, region 0 is the default
2.            press prefix+x      that region's current window receives WM_CLOSE
```

---

## Close semantics

Sends a standard Win32 `WM_CLOSE` message:

- **Asynchronous**: returns immediately after dispatch; the app decides when to actually close.
- **The app may intercept**: document apps with unsaved changes (Office / VSCode / Notepad) may pop a "Save?" dialog; if the user clicks Cancel, the window doesn't close.
- **The app may ignore it**: a few apps (some games / daemons) simply don't respond to `WM_CLOSE`, in which case `prefix+x` looks like nothing happened.

> If you want to forcibly kill the app process (bypassing `WM_CLOSE`), use Task Manager / `Stop-Process`. gmux doesn't offer that destructive capability.

---

## What happens after closing

- The temporary fullscreen flag for that region is cleared.
- The engine does **not** automatically place a new window into that region.
- To "auto-promote the next Chrome window": use `prefix+f`.
- To "re-place the entire layout": use `prefix+r`.
- The region itself doesn't disappear (except when a region produced by split is cleaned up under certain paths).

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| No layout currently available | Silent |
| selected_region has no window in that slot | Silent; also clears the stale fullscreen flag for that region |
| The window is already dead | Silent |

---

## What if I close the wrong thing

`prefix+x` follows the app's own close flow. There is **no undo**.

- If the app supports "reopen recently closed" (Ctrl+Shift+T in browsers, IDE "reopen closed"), use the app's native feature to restore.
- If it's a document app, clicking Cancel on the "Save?" dialog avoids the accidental close.
