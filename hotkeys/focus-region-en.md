---
title: prefix+g — Focus region
---

# prefix+g — Focus the selected region without moving the window

> **Language** · [中文](focus-region-zh.md) · **English** · [日本語](focus-region-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+g` jumps focus to the current window of the selected region. It **does not** move / resize / fullscreen anything. It also moves the mouse cursor onto that window's title bar.

> Default chord: `g` · Config key: `focus_region` · Change in Settings → Hotkeys → Region actions

---

## Trigger flow

```
1. press prefix+q N        ← you must select a region first, otherwise silent
2. press prefix+g          ← focus + mouse jumps to region N's window
```

---

## How it differs from other "jump focus" actions

| Action | Moves window | Fullscreen | Mouse position |
|---|---|---|---|
| `prefix+f` | Yes (rotates to the next one) | Inherits | Unchanged |
| `prefix+z` | Yes (maximize or restore) | Toggles | Unchanged |
| `prefix+g` | **No** | No | **Moves to the target's title bar** |
| Alt+Tab | No | No | Unchanged |

`prefix+g` is for "I'm watching a video on display A; press prefix+g to jump focus to the IDE on display B" — keep the visual layout but switch input focus. The mouse follows to the title bar so apps that need hover (Office toolbars, browser address bar) show their interactive state correctly.

---

## Rejection conditions

`prefix+g` is **stricter** than other region actions — it does not fall back to region 0:

| Situation | Behavior |
|---|---|
| No selected_region | Silently refused (you must `prefix+q N` first) |
| selected_region has no window in that slot | Silent |
| The window is already dead | Silent |
| No layout currently available | Silent |

> If you want to "focus region 0 no matter what", press `prefix+q 0` first, then `prefix+g`.

---

## Interaction with the system focus-stealing protection

Windows refuses external focus stealing under certain conditions (shortly after startup, when the active app is unresponsive, when a UAC prompt is showing, etc.) and only flashes the window's taskbar icon. If you hit this, click the desktop or the gmux Settings window first to reset focus, then press `prefix+g`.
