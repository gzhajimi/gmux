---
title: prefix+m — Swap displays
---

# prefix+m — Swap display layouts

> **Language** · [中文](swap-displays-zh.md) · **English** · [日本語](swap-displays-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+m` **shifts the entire current layout** to another display: every region originally on display A (windows, categories, apps and all) moves to display B, and vice versa.

> Default chord: `m` · Config key: `swap_displays` · Change in Settings → Hotkeys → Region operations

---

## Four usage patterns

After `prefix+m` enters swap mode, what you press next determines which of four branches you take:

| Next key | Branch | Use case |
|---|---|---|
| Press `m` again | **Quick swap (mm)** | Swap the two displays configured as quick_swap in the display profile; great for two displays you frequently flip between |
| Digit `N1` then digit `N2` | **Explicit swap (m+digit+digit)** | Swap two displays by their explicit display numbers; needed when you have three or more displays |
| Digit `N` then `Enter` | **Swap with default (m+digit+Enter)** | Swap the chosen display with the default display; only one number to remember |
| Digit `N` then `o` | **Open on display (m+digit+o)** | Open the app picker on display `N` (0-based number shown on the OSD) and set it as the sticky target for future `prefix+o` presses; see [prefix+o — Open window](open-window-en.md) |
| `Esc` / any other key / nothing | **Cancel** | Close the OSD without swapping |

The "display-number OSD" (each display shows its number + resolution in the center) only appears **120 ms** after entering swap mode. This delay means users hitting `mm` quickly **never see the OSD flicker**.

---

## Pattern 1: quick swap (`prefix+m m`)

The most common pattern. **No need to remember numbers**; press m twice to swap.

### Which two displays?

- If the currently active **display profile** has `quick_swap` set (in Settings → Display Profiles, the editor's "prefix+mm display pair" field at the bottom), those two are swapped.
- If quick_swap isn't set, the default is to swap the **first two displays** in the profile (list indices 0 and 1).
- If fewer than 2 displays are connected → silently refused.

### OSD behavior

- **Within 120 ms `mm`**: the OSD hasn't shown yet, the entire flow is **completely free of visual disruption**.
- **More than 120 ms before the second m**: the OSD has already appeared; the engine hides it before performing the swap to avoid the "single flash".

---

## Pattern 2: explicit swap (`prefix+m N1 N2`)

Press `prefix+m`, wait 120 ms for the display-number OSD, then press two digits.

```
press prefix
press m              ← enters swap mode
wait 120 ms          ← OSD appears, each display shows its number
press 1              ← first display (src)
press 2              ← second display (dst); swap fires immediately
```

### Reading the numbers

Each display's center shows a number (starting from 0) and resolution on the OSD. These numbers also match the numbers shown when you press "Re-detect" on the Displays page.

### What if the first digit comes very fast

If you press the first digit within 120 ms (for example, `prefix m 1 2` typed quickly), the engine **immediately sends the OSD** so you can see the highlight clearly, then waits for the second digit.

### Timeout for the second digit

After the first digit, there's no separate timeout — the entire swap mode shares one overall timeout (default `cycle_timeout_ms = 3000 ms`). You have 3 seconds from the `prefix+m` press to finish typing both digits.

---

## Pattern 3: swap with the default display (`prefix+m N Enter`)

Press `prefix+m`, wait 120 ms for the display-number OSD, press a single digit `N`, then press `Enter`. Display `N` is swapped with the **default display** — no second number to type.

```
press prefix
press m              ← enters swap mode
wait 120 ms          ← OSD appears, each display shows its number
press 1              ← the display to move
press Enter          ← swap display 1 with the default display
```

### Which display is the "default display"?

Resolved from your display profiles: a profile that ticks "use the first display as default" (`first_as_default`) **and** exactly matches the currently connected displays nominates its first display. Otherwise the default is display 0.

### Bare Enter

Pressing `Enter` before any digit cancels swap mode (same as `Esc`); the key is **not** passed through to the focused window.

### Rejection conditions

Same as explicit swap, all logged with no extra toast: `N` out of range, `N` is already the default display (source == destination), no layout has been triggered, or orientation mismatch. Shares the one overall `cycle_timeout_ms` (3 s) from `prefix+m`.

---

## Pattern 5: open app on a specific display (`prefix+m N o`)

After pressing `prefix+m` and waiting for the OSD, press a single digit `N` then press `o`. The [app picker](open-window-en.md) opens on display `N` immediately. Once you confirm an app in the picker, the window is placed on display `N`.

**Sticky override**: the engine remembers display `N` as the target for all subsequent `prefix+o` presses. Plain `prefix+o` then opens on display `N` instead of the default last display. The override is cleared on restart, display hot-plug / re-enumeration, or the next `prefix+m+digit+o` sequence.

**Out-of-range digit**: if `N` exceeds the number of connected displays a "no such display" toast is shown and the sequence is cancelled with no side effects.

---

## Pattern 4: cancel

Ways to cancel after entering swap mode:

- Press `Esc`: cancel; OSD closes.
- Press any key that isn't a digit, m, or Esc: cancel; OSD closes; **the key is passed through to the focused window**.
- Wait 3 seconds (`cycle_timeout_ms`): timeout cancel.
- Foreground window switch, display hot-plug, config reload: auto-cancel.

---

## Rejection conditions

Even with correct syntax, the engine refuses to perform the swap in these cases; the OSD closes and the reason is logged:

| Situation | Behavior |
|---|---|
| No layout has ever been triggered (current_layout is empty) | Refused |
| A display number exceeds the connected display count (e.g. three displays only have 0/1/2 and you pressed 3) | Refused |
| The two numbers are the same (e.g. `m 1 1`) | Refused |
| **Orientation mismatch**: a region to be swapped uses a landscape category, but the target display is portrait (or vice versa) | Refused with a warning |

Orientation mismatch is the most common rejection. To work around it: write an additional variant for the binding suited to portrait on the Layouts page, or align the orientations of both displays in the Display Profiles page.

---

## Relationship to other actions

- After a swap, the `prefix+f` cycle session is cleared (you need to re-select a region before continuing to cycle).
- Swap does not affect the region selected via `prefix+q` (the binding-scoped memory is independent of display order).
- Swap **only swaps the current layout**: other bindings still follow their own variants the next time they fire.

---

## Timing parameters (advanced)

| Parameter | Default | Settings location |
|---|---|---|
| Swap total timeout | `3000 ms` | Hotkeys → Advanced → Cycle timeout (`cycle_timeout_ms`) |
| OSD delay | `120 ms` | Built-in constant; not adjustable |
