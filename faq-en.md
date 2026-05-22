---
title: gmux FAQ
---

# gmux FAQ

> **Language** · [中文](faq-zh.md) · **English**

← [Back to homepage](index.md)

---

### Hotkey doesn't fire after editing the config

- Tray → **Reload Config**, or restart gmux.
- Look at the tray icon: red warning badge = your prefix is held by another app; right-click tray → **Retry prefix**.
- Tray → Settings will show TOML parse errors with line numbers + expected format.

### Variant doesn't switch when I (un)plug a monitor

gmux listens for display changes (`WM_DISPLAYCHANGE`, debounced ~500 ms): plug or unplug a monitor and it automatically re-enumerates displays and recompiles bindings, so the next trigger picks the variant matching the new layout — no manual reload needed. Windows already placed before the change aren't auto-rearranged; press the binding again, or `prefix+r`, to re-place them.

### How are two same-model monitors distinguished?

By EDID serial number. Each `[[display]]` entry's `id` field looks like `<model>@SN-<serial>` and is auto-written by the GUI's "Detect monitors" wizard.

Conceptual detail: [User guide → Display](settings-help-en.md#1-display).

### gmux can't find my app's window — what now?

Add explicit match rules under `[apps.<key>.match]`:

```toml
[apps.terminal]
launch.exe = "C:/Users/<you>/AppData/Local/Microsoft/WindowsApps/wt.exe"
match.hard.class = "CASCADIA_HOSTING_WINDOW_CLASS"   # Windows Terminal's window class

[[apps.terminal.match.soft]]
kind = "title_contains"     # nudge toward a specific tab
pattern = "PowerShell"
weight = 50
```

`match.hard.*` fields (`process` / `process_path` / `class` / `aumid`) are exact requirements; `[[apps.<key>.match.soft]]` entries are scored hints (`kind` = `title_contains` / `title_regex` / `class_prefix` / `process_prefix`, `weight` in −100…100).

The GUI's "Add app" wizard captures live window metadata so you don't have to write it by hand.

### Can I sync my config across machines?

`%APPDATA%\gmux\config.toml` is plain TOML — drop it in `git` / Dropbox / OneDrive.

Caveat: EDID serial numbers in `display_profiles` are **machine-specific**. Switching machines means either re-running "Detect monitors" on the new box, or pre-naming both machines' equivalent monitors with the same `name` so binding configs stay portable.

### Is the GUI mandatory?

No. `config.toml` works directly (reload to apply). The GUI is an alternative, not a requirement.

### Still stuck?

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) — feature requests, bug reports, and config questions all go through Issues so other users can find the same answers.
