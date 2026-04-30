---
title: prefix key
---

# prefix key (default Ctrl+M)

> **Language** · [中文](prefix-zh.md) · **English** · [日本語](prefix-ja.md)

[← Back to Settings Help](../settings-help-en.md)

prefix is the master switch for every gmux hotkey. Every built-in action and binding trigger goes through prefix first, then a second key.

---

## Default value and how to change it

- Default: `Ctrl+M`
- To change: **Settings → Hotkeys → prefix key** click the edit button, press a new combination, then press confirm.
- Must include at least one modifier (Ctrl / Alt / Shift / Win). Bare letters/digits, Esc, F-keys are rejected.
- Win is disabled by default. To use Win as a modifier, first tick the "Enable Win key as modifier" checkbox, then capture a new combination.
- Changes take effect immediately; no restart needed.

---

## How it works

Pressing prefix puts the engine into **wait-for-chord** state:

1. The engine briefly intercepts the global keyboard, waiting for the next key.
2. This wait window is **2000 ms** by default (change at Hotkeys → prefix key → PREFIX timeout).
3. The next key received during the wait determines what happens next:
   - It's the chord of a built-in action (defaults `n/p/l/r/f/q/z/m/space/w/g/c/x/?`) → fire that action.
   - It's the key of a binding (`0-9` or `a-z`) → fire that binding.
   - It's `Esc` → silent cancel.
   - It's any other key → cancel the wait and **pass through** that key to the focused window (it isn't swallowed).
   - No key pressed before timeout → silent cancel.
- Pressing modifier keys alone during the wait (Ctrl / Shift / Alt / Win / Caps Lock) **does not** cancel the wait; you can keep typing the chord.

---

## Triggering a binding (prefix + binding key)

Each binding's trigger key is shown in the left column of the Layouts page as `⌃M·1`, `⌃M·a`, etc. (the prefix portion reflects the current prefix).

- `prefix` `<0-9>` → fire a digit-key binding
- `prefix` `<a-z>` → fire a letter-key binding

After you press it:

- The engine picks the **uniquely matching** variant for the currently connected physical displays out of that binding's variants and runs it.
- No matching variant → silently dropped, with a silent toast if appropriate (see below).
- Already-running app windows are **moved** into the target slots; not-yet-running apps are launched per the variant's launch method.
- After firing, that binding becomes the "current layout"; `prefix+r`, `prefix+n/p/l`, `prefix+w`, etc. all operate relative to the current layout.

---

## What if prefix is taken

If another app (an AutoHotkey script, OneNote, WeCom, QQ screenshot, etc.) grabbed the same combination:

- The tray icon turns into a **red warning**.
- Right-click the tray → **Retry prefix**: re-register after you close the offending app.
- Or go to Settings → Hotkeys and pick a non-conflicting combination.

---

## Advanced: prefix self-heal on inactivation

Low-level keyboard hooks occasionally get silently unloaded by Windows when the system is busy. After two consecutive prefix waits receive zero key events, gmux automatically reinstalls the hook, and the next prefix press recovers. This self-heal is transparent to the user; no manual action needed.

---

## Silent toasts

When the **Hotkeys → Silent operation toast** switch is on, pressing prefix + chord but failing **the precondition** (no layout matches the current displays, the region is too small for `split_region`, no region is selected for `focus_region`, etc.) shows a short one-line toast in the center of the default display. The same toast only fires once per 3 seconds.

When the switch is off, every "precondition not met" case is completely silent and only shows up in the log.

---

## Interaction with the Settings window

- When the Settings window is focused, `F1` opens the Help page directly (equivalent to `prefix+?`).
- The Settings window's **About** and **Open-source licenses** pages don't respond to F1 (to avoid interrupting reading).
