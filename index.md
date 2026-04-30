---
title: gmux — declarative workspace switcher
---

[中文](zh.md) · English · [GitHub](https://github.com/gzhajimi/gmux)

# gmux

**A declarative workspace switcher for Windows.**
Describe multi-monitor layouts in TOML. Press a hotkey. Snap.

### How it works

1. **Describe** your layouts in a TOML file — windows, monitors, regions.
2. **Press `Ctrl+M` + a key** — your chord (`1`, `2`, `q`, …).
3. **Windows snap into place** — already-open apps are *moved*, not relaunched.

## Get gmux

**⭐ [Star the repo on GitHub](https://github.com/gzhajimi/gmux) to be notified when the first release ships.**

| Channel | Best for | Status |
|---|---|---|
| **Microsoft Store** | Auto-update, premium features, regular users | Pending review |
| **GitHub Release** (NSIS installer) | China, enterprise networks, git-tracked configs | First release pending |

**System requirements:** Windows 10 build 17763 (1809) or later, 64-bit.

## See it in TOML

A binding that fills your main display with Notepad — and reuses the same window the next time you press it.

```toml
[hotkey]
prefix = "Ctrl+M"

[apps.notepad]
exe = "C:/Windows/System32/notepad.exe"

[[display]]
id = "__PRIMARY__"
name = "main"
orientation = "landscape"

[display_profiles.solo]
display = [{ name = "main" }]

[bindings.1]
description = "Notepad fullscreen"
[[bindings.1.variants]]
profile = "solo"
categories = { main = "full" }
regions = [{ on = "main", slot = "full", app = "notepad" }]
```

`Ctrl+M` then `1` → Notepad fills your main monitor. Press it again → the same window is moved, not relaunched.

## Why gmux

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

The gmux binary is distributed under its respective Microsoft Store / GitHub Release terms. This documentation site is published under the [MIT License](LICENSE).
