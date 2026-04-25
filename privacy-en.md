---
title: gmux Privacy Policy
lang: en-US
---

# gmux Privacy Policy

Last updated: 2026-04-25

## TL;DR

gmux does not collect any personal data. It runs entirely on your local machine and does not phone home — except to query your Microsoft Store license status.

## Details

### What we do NOT collect

- Personally identifiable information (name, email, phone)
- Usage telemetry, crash reports, analytics
- Your window titles, app names, or file paths
- Any form of behavioral tracking

### Data stored on your machine

- `%APPDATA%\gmux\config.toml` — your scene definitions
- `%APPDATA%\gmux\license.toml` — license state cache (Store build only)

These files never leave your machine. On uninstall, you choose whether to keep them.

### Network activity

gmux makes only these network calls, all via the Windows-provided `Windows.Services.Store` API:

- On startup: check if your Microsoft account has purchased the Premium add-on
- On purchase: invoke the system Store purchase dialog
- Every 6 hours: silently refresh license state

gmux does not contact any third-party servers directly.

### Third parties

- **Microsoft Store** handles purchases and licensing. See <https://privacy.microsoft.com>

### Contact

For questions or bug reports, please open an issue: <https://github.com/gzhajimi/gmux/issues>
