---
title: gmux — declarative workspace switcher
---

[中文](zh.md) · English · [日本語](ja.md) · [GitHub](https://github.com/gzhajimi/gmux)

# gmux

**A declarative workspace switcher for Windows.**
Describe your multi-monitor layouts in the settings UI. Press a hotkey. Snap.

## How it works

1. **Describe** your layouts in gmux's settings UI — monitors, apps, regions. All graphical, no config file to hand-edit.
2. **Press `Ctrl+M` + a key** — your chord (`1`, `2`, `q`, …).
3. **Windows snap into place** — already-open apps are *moved*, not relaunched.

## Get gmux

**gmux is available now on the Microsoft Store** — automatic updates, the 30-day Premium trial, no account required.

[**Get gmux on the Microsoft Store →**](https://apps.microsoft.com/detail/9MWP3799MCLH)

A **macOS** version is in preparation. [Star the repo on GitHub](https://github.com/gzhajimi/gmux) to hear when it ships.

**System requirements:** Windows 10 build 17763 (1809) or later, 64-bit.

## See it in 30 seconds

A few clicks in the settings UI configure a binding that fills your main display with Notepad: pick the monitor, choose a layout, drop an app into a region.

`Ctrl+M` then `1` → Notepad fills your main monitor. Press it again → the same window is moved, not relaunched.

**Your config is portable.** Everything you set up in the UI is saved as a declarative TOML file. You never have to write it by hand, but it's plain text — back it up, sync it across machines with git / Dropbox / OneDrive, and reuse it on a new PC:

```toml
[bindings.1]
description = "Notepad fullscreen"
[[bindings.1.variants]]
profile = "solo"
categories = { main = "full" }
regions = [{ on = "main", slot = "maximize", app = "notepad" }]
```

## Why gmux

- **Graphical setup** — monitors, layouts, apps, and hotkeys are all configured in the settings UI; no config file to hand-write.
- **Declarative** — describe the destination, not the steps.
- **Non-destructive** — running apps move, they don't relaunch.
- **Prefix-key** — leaves the global hotkey namespace untouched.
- **Lightweight** — < 10 MB installer, < 50 MB idle RAM.
- **100% local** — no cloud, no account, no subscription, no telemetry.

gmux is **not** a tiling WM (yabai / komorebi), **not** snap zones (MaxTo), **not** a workflow recorder (Keyboard Maestro). It does one thing: make scene switches the lowest-friction action in your day.

## Free vs Premium

**Free** — forever, no ads, no telemetry, no account required: up to 9 bindings, one display profile, single-display variants.

**Premium** — one-time purchase, lifetime license: unlimited bindings, unlimited profiles (home / office / travel), multi-display variants.

**30-day trial** — the first time you hit a Premium feature, a dialog offers Trial / Buy / Stay free. No signup. One trial per machine.

## Documentation

- [User guide](settings-help-en.md) — five core concepts, every built-in hotkey, settings walkthrough
- [FAQ](faq-en.md) — hotkey not firing, multi-monitor IDs, cross-machine sync
- [Privacy policy](privacy-en.md)

## Feedback

Bug reports, feature requests, and config questions all go through [GitHub Issues](https://github.com/gzhajimi/gmux/issues) — that way other users can find the same answers.

---

The gmux binary is distributed under its Microsoft Store terms. This documentation site is published under the [MIT License](LICENSE).
