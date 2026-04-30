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

The engine evaluates variants only on launch / reload. Hot-plug currently requires a manual reload. Listening to `WM_DISPLAYCHANGE` for automatic re-evaluation is on the roadmap.

### How are two same-model monitors distinguished?

By EDID serial number. Each `[[display]]` entry's `id` field looks like `<model>@SN-<serial>` and is auto-written by the GUI's "Detect monitors" wizard.

Conceptual detail: [User guide → Display](settings-help-en.md#1-display).

### gmux can't find my app's window — what now?

Specify `[apps.<key>.window_identity]` explicitly:

```toml
[apps.terminal]
exe = "C:/Users/<you>/AppData/Local/Microsoft/WindowsApps/wt.exe"
[apps.terminal.window_identity]
class = "CASCADIA_HOSTING_WINDOW_CLASS"     # Windows Terminal's class
title_contains = "PowerShell"                # narrow further to a specific tab
```

The GUI's "Add app" wizard captures live window metadata so you don't have to write it by hand.

### Can I sync my config across machines?

`%APPDATA%\gmux\config.toml` is plain TOML — drop it in `git` / Dropbox / OneDrive.

Caveat: EDID serial numbers in `display_profiles` are **machine-specific**. Switching machines means either re-running "Detect monitors" on the new box, or pre-naming both machines' equivalent monitors with the same `name` so binding configs stay portable.

### Is the GUI mandatory?

No. `config.toml` works directly (reload to apply). The GUI is an alternative, not a requirement.

### Still stuck?

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) — feature requests, bug reports, and config questions all go through Issues so other users can find the same answers.
