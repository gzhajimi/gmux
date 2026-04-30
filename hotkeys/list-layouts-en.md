---
title: prefix+w — List all layouts
---

# prefix+w — List layouts and pick one

> **Language** · [中文](list-layouts-zh.md) · **English** · [日本語](list-layouts-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+w` brings up a grid OSD that lays out every layout available for the current display set side by side. Browse with the arrow keys, press Enter to switch.

> Default chord: `w` · Config key: `list_layouts` · Change in Settings → Hotkeys → Layout navigation

---

## Which display the OSD shows on

- By default it appears on the "default display".
- Default-display rules:
  1. If the currently matched display profile has "Treat the first display as the default" ticked, the OSD shows on the first display in that profile's list.
  2. Otherwise it shows on the OS primary display.
- The OSD auto-closes after 30 seconds by default (change at Hotkeys → Advanced → LAYOUT LIST OSD limit).

---

## Browsing: hjkl moves the cursor

After the OSD appears, use vim-style hjkl to move within the grid:

| Key | Action |
|---|---|
| `h` | Cursor left |
| `l` | Cursor right |
| `k` | Cursor up |
| `j` | Cursor down |
| `Space` | Toggle the detail view (see below) |
| `Enter` | Fire the layout under the cursor |
| `Esc` | Cancel, close the OSD |
| **Mouse left/middle/right click** anywhere on screen | Cancel, close the OSD (the click is **not** swallowed; it still reaches the window underneath) |

Each press of hjkl or Space **resets the 30-second countdown**; Enter or Esc closes immediately.

---

## Detail view (Space toggles it)

By default each layout thumbnail shows only "binding description + display count". For overly complex layouts (multi-display + many apps), the thumbnail can't fit everything:

- **Auto detail**: when the cursor lands on such an "overflow" layout, a strip below automatically expands to show the full app list.
- **Manual toggle**: press `Space` to flip between "follow auto" and "force the inverse". That is:
  - Default not overflowed → manual Space forces it to expand.
  - Default auto-expanded → manual Space forces it to collapse.
- Moving the cursor to another layout returns to that layout's "auto detail" state (the manual flip is reset).

---

## Confirm and cancel

- `Enter`: fire the layout under the cursor; OSD closes. That layout becomes the new "current layout".
- `Esc`: cancel; OSD closes; no layout switch.
- Mouse click: **equivalent to Esc**. The click is not swallowed and lands on the window underneath as usual (so the workflow doesn't get interrupted by a stray click).
- Wait 30 seconds (default): auto-cancel.

---

## Layouts that won't appear

The OSD only lists **layouts whose variants match the current display set**. The following bindings **will not appear**:

- No variant matches the currently connected physical displays.
- Bindings locked under the free tier (10th onward when you have more than 9, or multi-display variants on free).

If no layouts are available at all:

- The OSD doesn't appear.
- When Hotkeys → Silent operation toast is enabled, a "no layouts available" toast is shown.

---

## OSD ordering

The order on the OSD matches the order in the left column of the Layouts page. Drag bindings on the Layouts page to reorder.

---

## Relationship to other actions

- A layout fired via `prefix+w → Enter` is equivalent to a direct `prefix+<key>` trigger: it is recorded in the `last_layout` history and can be jumped back to with `prefix+l`.
- Cancellation (Esc / mouse / timeout) does not record into history.
- During the OSD, any "external event" (foreground window switch, display hot-plug, config reload) immediately cancels the OSD.

---

## Timing parameters

| Parameter | Default | Settings location |
|---|---|---|
| OSD auto-close time | `30000 ms` | Hotkeys → Advanced → LAYOUT LIST OSD limit (`layout_list_timeout_ms`) |
