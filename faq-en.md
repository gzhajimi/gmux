---
title: gmux FAQ
---

# gmux FAQ

> **Language** · [中文](faq-zh.md) · **English** · [日本語](faq-ja.md)

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

Fix it on the **Settings → App Match** page — no hand-editing required:

1. Tray → **Settings** → sidebar **App Match**.
2. Find the app in the list and click **"Re-match"**. gmux launches and samples its window automatically, extracts a stable match fingerprint (process / window class / title, …), and writes it back to the config — you don't need to open the app yourself first.
3. If it grabs the wrong window, or can't extract a stable fingerprint, a **"Pick a window"** dialog appears: choose which running window is this app and confirm (this step needs the target window already open).

Each app on the page shows its current status ("Running · N windows matched" / "Not running") and what it "Matches by". You normally never touch it — only when a placement grabs the wrong window or finds none do you click "Re-match".

> **Terminal apps:** PowerShell / cmd / WSL running inside Windows Terminal actually belong to `WindowsTerminal.exe`. In the "Pick a window" dialog, search `terminal` or leave it blank to browse, then pick the right row by the process name shown under each entry.

### Can I sync my config across machines?

`%APPDATA%\gmux\config.toml` is plain TOML — drop it in `git` / Dropbox / OneDrive.

Caveat: EDID serial numbers in `display_profiles` are **machine-specific**. Switching machines means either re-running "Detect monitors" on the new box, or pre-naming both machines' equivalent monitors with the same `name` so binding configs stay portable.

### Is the GUI mandatory?

No. `config.toml` works directly (reload to apply). The GUI is an alternative, not a requirement.

### Still stuck?

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) — feature requests, bug reports, and config questions all go through Issues so other users can find the same answers.
