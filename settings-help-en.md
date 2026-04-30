---
title: gmux Settings Help
---

# gmux Settings Help

> **Language** · [中文](settings-help-zh.md) · **English** · [日本語](settings-help-ja.md)

This document explains gmux's core concepts from the perspective of the **Settings window**, plus the usage and effect of every built-in hotkey. Open the tray icon → "Settings" to find every page mentioned below.

The sidebar, top to bottom: **Displays / Display Profiles / Layouts / Hotkeys / System / Help**. These six pages match the workflow "tell gmux which displays I have" → "group displays into named profiles" → "write layouts for those profiles and bind them to hotkeys" → "tweak the trigger keys".

---

## 1. Key concepts

Everything gmux does revolves around five concepts. They depend on each other like this:

```
Display ── registered into ──▶ Profile
                                 ▲
                                 │ references
                                 │
Binding ── contains many ──▶ Variant
                                 │
                                 └── references ── layout category + slot
                                 └── references ── app
```

Trigger order: **press the binding's hotkey → the engine picks one variant matching the current displays from that binding's variants → the apps declared in that variant are placed into the corresponding slots**.

---

### 1. Display

Corresponding settings page: **Displays**.

A "Display" is gmux's named registration for a physical screen. On the very first launch after install, gmux auto-detects every connected display and registers them.

Each display record contains:

| Field | Meaning |
|---|---|
| `id` | Physical ID (EDID serial + model); used to distinguish multiple displays of the same model. `__PRIMARY__` is the placeholder for "the current OS primary". |
| `name` | The name you give it (e.g. `main`, `vertical`, `laptop`); display profiles and variants reference this name. |
| `orientation` | Orientation: `landscape` / `portrait`. |

**Why register a name instead of using the physical ID directly**: after a hardware swap or display replacement, register a display with the same name and every reference still works. The same display rotated counts as a different record (you can register a landscape and a portrait version separately).

Page actions:

- **Re-detect**: refresh the connected physical display list, briefly drawing each display's number on its surface to help you identify them.
- **Hide display numbers**: manually dismiss that number OSD.
- **Unnamed card → Register**: type a name and save; the display moves into the "registered" section.
- When **referenced by some display profile**, the card cannot be edited or deleted directly; unbind on the Display Profiles page first.

---

### 2. Display Profile

Corresponding settings page: **Display Profiles**.

A "Display Profile" = an ordered set of registered displays, representing a single **physical usage environment**. For example:

- `solo` — primary display only
- `home` — primary + portrait display on the right
- `office` — left display + primary + laptop on the right

Every binding's variant must hang off some profile. When firing a binding, the engine picks the variant whose profile matches the **currently connected displays**.

Each profile record contains:

| Field | Meaning |
|---|---|
| `name` | Profile name (e.g. `home`) |
| `displays` | Ordered list of displays, persisted in drag order; the number you see in the UI is the list index |
| `first_as_default` | Treat the first display in the list as the "default display"; affects the prefix+w panel and the split fallback target |
| `quick_swap` | Optional pair of displays used by the prefix+mm hotkey for swapping |

Page actions:

- **New display profile**: drag a set from the registered-displays pool into the preview area; reorder them to match physical placement.
- **Drag profile cards to reorder**: determines the order in which display profiles appear in the OSD list.
- When loading config the engine compares against currently connected physical displays. Matched profiles show as "online"; otherwise a lock icon explains which binding references them and that they're temporarily uneditable.

---

### 3. Binding

Corresponding settings page: **Layouts** (each item in the left column is a binding).

A "Binding" = a **hotkey** + description + a set of variants. The binding's key is one of the digits `0-9` or letters `a-z`. Pressing `prefix + <key>` fires it (the default prefix is `Ctrl+M`, so it's something like `Ctrl+M 1` / `Ctrl+M a`).

Each binding contains:

| Field | Meaning |
|---|---|
| `id` | Trigger key, e.g. `1` or `a` |
| `description` | Binding description (the title shown in the left column, e.g. "Dev", "Meeting", "Single-display reading") |
| `variants` | One or more variants; the engine picks one to run based on the current display configuration |

Page actions:

- **New binding**: enter a description + pick a trigger key. Already-used trigger keys are greyed out in the picker.
- **Delete binding**: removes the binding along with all its variants; the trigger key becomes available again.
- **Rename**: the ✎ icon at the top right edits description; the trigger key stays the same.
- **Reorder the left column**: determines the order in which bindings appear in the prefix+w list.

The free tier is capped at **9 bindings**; beyond that requires upgrade or trial.

---

### 4. Variant

Corresponding settings page: **Layouts** (the "variant cards" in the right-hand detail area).

A "Variant" = the concrete layout description for the same binding **under a particular display profile**. One binding can hold multiple variants, each tied to a different profile, and the engine auto-selects based on the currently connected displays.

> Example: the binding "Dev" can carry three variants — dual-display `home` uses the left display for IDE and the right for docs; triple-display `office` uses left for IDE / middle for Chrome / right for Slack; single-display `solo` uses the primary fullscreen for IDE. When you take the laptop out, only `solo` matches, and `Ctrl+M 1` runs the solo variant.

Each variant contains:

| Field | Meaning |
|---|---|
| `profile` | The display profile this variant is bound to |
| `categories` | The **layout category** assigned to each display in the profile (landscape / portrait each have 12 built-in categories + the universal `full`) |
| `regions` | App-to-slot mapping: which app goes into which slot of which display |

**Layout category + slot** is a geometry template:

- The category determines how a display is divided (e.g. `quad_h` = four quadrants, `halves_h` = left/right halves, `thirds_v` = three vertical bands).
- A slot is the name of one sub-region after dividing (e.g. `tl/tr/bl/br`, `left/right`, `top/middle/bottom`).
- One display picks one category, and the category determines what slots are available to fill.
- 25 built-in categories total (landscape 12 + portrait 12 + universal `full`); not customizable — keeping them fixed lets the GUI editor always "recognize" the config.

Page actions:

- **Attach a display profile**: in the binding detail area click +, create a variant; pick a profile → assign a category to each display → drag apps into slots.
- **Edit**: the "Edit" icon at the top right of a variant card opens the drawer.
- The currently active variant is marked "Active" on its card; variants that don't match the currently connected displays are marked "Not matched".
- Apps can be picked from the "registered" section, or from the system Start Menu / UWP list; the first time an app is picked it's auto-recorded into the apps registry.

Free-tier limit: each variant can only involve one display; multi-display variants require Premium.

---

### 5. The whole thing in one paragraph

> **Which displays a machine has → register them as "Displays"**.
> **A few displays form one usage environment → name it as a "Display Profile"**.
> **Want a hotkey for one work posture → create a "Binding"**.
> **Same binding does different things in different profiles → write multiple "Variants"**.
> **Each variant describes how each display is divided and who goes where → category + slot + app**.

---

## 2. System preset hotkeys

Corresponding settings page: **Hotkeys** (the left column has three sections: "prefix key", "Action hotkeys", "Advanced").

gmux is a **prefix + chord** system: press prefix first to enter "wait for second key" state, then press a key to fire an action. Prefix defaults to `Ctrl+M`; every built-in action requires prefix first.

> The "Settings → Hotkeys" page lets you change prefix, change each action's second key, and adjust timeouts; the values listed below are the **first-launch defaults**.

> **Each item has its own detailed doc** describing every possible usage branch, cancel condition, and rejection condition. Click the action name in the table below to jump.

### 2.1 Prefix

| Setting | Default | Effect |
|---|---|---|
| [`prefix`](hotkeys/prefix-en.md) | `Ctrl+M` | The master switch that enters "wait for chord" state |
| Enable Win key as modifier | Off | Tick to allow Win to be a prefix modifier |
| PREFIX timeout | 2000 ms | Max time to wait for the second key after prefix |

**How to change**: in "Hotkeys → prefix key", click the edit button and press a new combination to save. Prefix must include at least one modifier (Ctrl / Alt / Shift / Win); bare letters or digits are rejected.

> Tip: if the tray icon turns red, the prefix has been hijacked by another app; pick a different combination or close the offender first, then click "Retry prefix" in the tray.

Full doc: [hotkeys/prefix-en.md](hotkeys/prefix-en.md)

### 2.2 Trigger a binding

After pressing prefix, type the binding key to fire the corresponding layout:

| Key sequence | Effect |
|---|---|
| `prefix` `<0-9 / a-z>` | Fires `bindings.<key>` — the engine picks a matching variant and places windows accordingly |

A binding's current key shows in the left column of the Layouts page (e.g. `⌃M·1` means Ctrl+M then 1). For details on binding-trigger semantics (including prefix-mode cancellation) see [hotkeys/prefix-en.md](hotkeys/prefix-en.md).

### 2.3 Layout navigation actions

Page section: "Hotkeys → Action hotkeys → Layout navigation".

| Action | Default key | Full sequence | Effect |
|---|---|---|---|
| [`next_layout`](hotkeys/next-layout-en.md) | `n` | `Ctrl+M n` | Switch to the "next" layout; press repeatedly to cycle through every binding |
| [`prev_layout`](hotkeys/prev-layout-en.md) | `p` | `Ctrl+M p` | Switch to the "previous" layout; reverse cycle |
| [`last_layout`](hotkeys/last-layout-en.md) | `l` | `Ctrl+M l` | Jump to the last-used layout (good for toggling between two layouts) |
| [`restore`](hotkeys/restore-en.md) | `r` | `Ctrl+M r` | Re-apply the current layout: clear temporary fullscreen, restore manually-dragged windows |
| [`show_desktop`](hotkeys/show-desktop-en.md) | `space` | `Ctrl+M Space` | Show desktop / restore (system-level Win+D equivalent, but routed through prefix) |
| [`list_layouts`](hotkeys/list-layouts-en.md) | `w` | `Ctrl+M w` | OSD lists every layout; pick with hjkl, Enter to confirm, Esc to cancel |

### 2.4 Region action hotkeys

Page section: "Hotkeys → Action hotkeys → Region actions".

A "region" = one slot of one display in the current variant. For example, the top-left of a four-quadrant layout, or the left half of a left/right split. These actions all need a region selected first, or the action specifies a region.

| Action | Default key | Full sequence | Effect |
|---|---|---|---|
| [`show_regions`](hotkeys/show-regions-en.md) | `q` | `Ctrl+M q <digit>` or `Ctrl+M q q <digits> Enter` | Paint every region's number on screen; press a digit to select (≥10 enters multi-digit input mode) |
| [`cycle_region`](hotkeys/cycle-region-en.md) | `f` | `Ctrl+M f` | In the selected region, rotate the next window of the current app into place |
| [`fullscreen`](hotkeys/fullscreen-en.md) | `z` | `Ctrl+M z` | Temporarily fullscreen the selected region; press again to restore (one fullscreen per display) |
| [`focus_region`](hotkeys/focus-region-en.md) | `g` | `Ctrl+M g` | Jump focus + mouse to the selected region's window (does not move the window) |
| [`split_region`](hotkeys/split-region-en.md) | `c` | `Ctrl+M c` then app picker | Add an app inside the selected region (region is split) |
| [`close_window`](hotkeys/close-window-en.md) | `x` | `Ctrl+M x` | Close the selected region's current window (asynchronous WM_CLOSE) |
| [`swap_displays`](hotkeys/swap-displays-en.md) | `m` | `Ctrl+M m m` or `Ctrl+M m <digit><digit>` | Swap the entire layout between two displays |

`swap_displays` has the most chord branches; the full three usages (mm / m+digit+digit / cancel) are documented in [hotkeys/swap-displays-en.md](hotkeys/swap-displays-en.md).

### 2.5 Help

| Action | Default key | Full sequence | Effect |
|---|---|---|---|
| [`open_help`](hotkeys/open-help-en.md) | `?` | `Ctrl+M ?` | Open the "Help / cheatsheet" window; in the engine `?` is equivalent to `/`, no Shift required |

When the Settings window is showing, `F1` also opens the Help page directly.

### 2.6 Advanced timing

"Hotkeys → Advanced" lets you adjust four ms-level timeouts and the launch concurrency cap:

| Setting | Default | Effect |
|---|---|---|
| Cycle timeout (`cycle_timeout_ms`) | 3000 ms | Max interval for repeated next/prev presses to count as the same cycle session |
| LAYOUT LIST OSD limit (`layout_list_timeout_ms`) | 30000 ms | Auto-close time for the `list_layouts` OSD |
| REGION number input timeout (`region_input_timeout_ms`) | 1000 ms | Max interval between two digits in `focus_region` / `swap_displays` |
| REGION OSD cleanup interval (`region_cleanup_interval_ms`) | 1000 ms | Internal cleanup tick for the region OSD |
| Max parallel launches (`max_parallel_launches`) | 0 (auto) | How many apps to cold-start in parallel for one binding trigger; 0 = auto-compute by CPU, 1–32 = fixed |

### 2.7 Silent operation toast

When "Hotkeys → Silent operation toast" is on, pressing a hotkey but failing the precondition (no available layout, region too small, no region selected, etc.) shows a short one-line toast in the center of the default display, so you don't think "nothing happened" when you were actually silently dropped. The same toast only fires once per 3 seconds.

### 2.8 One-click reset to defaults

The "Reset to defaults" button at the top right of the Hotkeys page resets prefix, every timeout, and all 14 action chords back to the first-launch defaults. If a default would collide with an existing binding key, the reset is refused with the conflicts listed so you can re-key manually first.

---

## 3. Other settings pages, briefly

| Sidebar entry | Main effect |
|---|---|
| **App matching (System page bottom, expand "Advanced")** | View each registered app's launch method and window-matching signal; re-match after a window's shape changes |
| **System → Language** | Change the UI language (follow system / 中文 / English / 日本語) |
| **System → Logs** | Set log level, directory, rotation policy; export the diagnostics zip |
| **Help** | Quick reference for the current prefix, every binding's trigger key, every action chord — handy after editing hotkeys |

---

## 4. Config file location

Every GUI change ultimately lands in one TOML:

```
%APPDATA%\gmux\config.toml
```

Editing this file directly and triggering **Tray → Reload config** also works; the GUI is "another entry point", not the only path. The config can be synced via git / Dropbox, but the EDID serials in `display_profiles` are **machine-specific** — across machines you need to re-detect displays or pre-assign matching `name`s on both machines.
