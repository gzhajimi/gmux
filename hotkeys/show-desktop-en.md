---
title: prefix+space — Show desktop
---

# prefix+space — Show desktop / restore

> **Language** · [中文](show-desktop-zh.md) · **English** · [日本語](show-desktop-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+space` toggles desktop visibility: minimize all windows ↔ restore them.

> Default chord: `space` · Config key: `show_desktop` · Change in Settings → Hotkeys → Layout navigation

---

## Behavior

- First press: all windows are minimized, exposing the desktop.
- Press again: every window that was just minimized is restored to its prior position.

Equivalent to the system's `Win+D`, just routed through prefix — handy if you don't want to dedicate a separate Win-key shortcut.

---

## Trigger flow

Press `prefix+space` directly; no second key required.

---

## Notes

- This is a **system-level** desktop toggle. It doesn't distinguish gmux-placed windows from anything else — all non-tray windows are minimized.
- Restore brings every window back at once. If you manually moved some windows while they were minimized, pressing `prefix+space` again may restore things in a way that doesn't quite match what you expect (this is Windows' own toggle-desktop behavior; gmux does nothing special here).
- Does not affect selected_region / the current layout / fullscreen flags.

---

## Rejection conditions

None. `prefix+space` always fires (it doesn't depend on any gmux state).
