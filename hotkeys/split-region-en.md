---
title: prefix+c — Add an app to a region
---

# prefix+c — Add an app inside a region (split)

> **Language** · [中文](split-region-zh.md) · **English** · [日本語](split-region-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+c` **splits the selected region in two** so you can add an app next to the original. The original app keeps one half; the new app takes the other.

> Default chord: `c` · Config key: `split_region` · Change in Settings → Hotkeys → Region actions

---

## Trigger flow

```
1. press prefix+q N      ← select region N (optional; see fallback below)
2. press prefix+c        ← the app picker popover appears
3. type an app keyword or pick a number
4. press Enter / digit / Shift+digit / Esc
```

---

## What if no region is selected

`prefix+c` does **not** fall back to region 0 when there is no selected_region; it uses the "default split target":

- Pick the "best fit" leaf among all visible leaves of the current layout: prefer the largest leaf on the default display.
- Default display = the first display in the profile when "Treat the first display as the default" is ticked; otherwise the OS primary display.

So in single-display, single-app scenarios, just press `prefix+c` to add an app on top of the primary display's layout — no need to select a region first.

---

## App picker popover

After it appears, the popover anchors above the target region (if the region is too low, it flips to above). Each row shows the app icon + name + the number of windows currently open.

### Selection methods

| Input | Action |
|---|---|
| Letters `a`–`z` | Append to the search box (live filter) |
| `Backspace` | Delete the last character in the search box |
| Digits `0`–`9` | Pick item 0 / 1 / … / 9 in the visible list |
| `Shift + digit` | Pick item N, **force-launch a new window** (even if the app is already running) |
| `Enter` | Pick the **first item** in the visible list |
| `Shift + Enter` | Pick the first item, **force-launch a new window** |
| `Esc` | Cancel; close the popover |
| Other keys | Silently absorbed (no cancel) |

> Digits go up to `9`, so only the first 10 visible items can be picked by digit. From the 11th onward you need to keep typing letters to filter, then use 0–9.

### Default behavior: reuse vs. launch new

- Digit / Enter (without Shift): **prefer reusing** an already-running window (pick the most recent by MRU); only launch if the app isn't running.
- Shift + digit / Shift + Enter: **force-launch a new window**. Good for Chrome / terminals where "I just want a new one" is the intent.

---

## Rejection conditions

Under any of the following, `prefix+c` does not show the popover and is silently refused (with a silent toast if appropriate):

| Situation | Toast text (when silent toast is enabled) |
|---|---|
| No layout currently available | `split_no_layout` |
| Selected region is not a leaf (it has already been split into an internal node) | `split_not_leaf` |
| Region is too small (after split, neither side could fit the minimum window size) | `split_too_small` |
| No registered apps in the config | `split_no_apps` |
| Pressed Enter when the search filter has no matches | `split_no_match` |

---

## Timing parameters

| Parameter | Default | Settings location |
|---|---|---|
| Popover timeout | `10000 ms` | Built-in `picker_timeout_ms`, reset on every key press |

> Every key press (digit, letter, Backspace, Space) **resets the 10-second timeout**. 10 seconds idle without any key press → auto-close.

---

## After the popover closes

- Picked a digit / pressed Enter → split executes immediately: the original region is split in half along its short edge; the original app stays, the new app moves into the new half.
- Cancel / timeout → no region is modified.
- After a successful split, selected_region is unchanged (still points at the original region's number). The new region's number is allocated by the engine and shows up in the next `prefix+q` OSD.

---

## Undoing a split

Split is a real layout change (live state) and **does not write back to TOML**. To undo:

- `prefix+r` (restore): re-applies the current layout per TOML; every split is discarded.
- Switching to another binding and back: the live state is reset by the binding's variant (splits go too).

If you want the post-split shape to be a permanent layout, edit that binding's variant on the Layouts page and add the new app as a new region.
