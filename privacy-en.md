---
title: gmux Privacy Policy
lang: en-US
---

# gmux Privacy Policy

Last updated: 2026-04-30

## TL;DR

gmux runs entirely on your local machine. It does not collect personal data, does not send telemetry, and does not contact any third-party server. The only data that ever leaves your device is a Microsoft Store licence query (product ID, no personal data) — and it goes through Windows' built-in `Windows.Services.Store` API.

## Scope

This policy covers the gmux desktop application distributed through the Microsoft Store.

- Package name: `D4681314.gmux`
- Publisher: 哈基米 (CN=727E2FF6-A413-46ED-8027-74D82A3CD038)

## What gmux reads on your computer (local, in-memory)

To place running windows into the layouts you defined, gmux reads window metadata from your local machine through standard Win32 APIs:

- Window titles and window class names
- Process names, executable paths, command lines
- Foreground-window change events (used to maintain a most-recently-used list, capped at 32 entries per app)

This information is processed in memory and is **never transmitted off the device**.

## What gmux stores on your machine

| File / location | Purpose |
| --- | --- |
| `%APPDATA%\gmux\config.toml` | Your scene and binding definitions |
| `%APPDATA%\gmux\license.toml` | Microsoft Store licence cache (Store build only) |
| `%APPDATA%\gmux\logs\gmux.log.*` | Rotated log files (errors and lifecycle events; rotation cadence and retention are configurable in settings) |
| Microsoft Store Startup Task `gmux_autostart` (Store build) — declared in the MSIX manifest, **disabled by default** | Auto-start on login. You turn it on; you can turn it off again from Windows' Startup Apps settings. |
| Named mutex `Global\gmux_mutex_v1` | Prevents two copies running simultaneously; carries no data. |

These files stay on your computer. They are not uploaded anywhere. Uninstalling gmux removes the executable; you can manually delete `%APPDATA%\gmux\` if you also want to remove the configuration and logs.

## What gmux does NOT collect or send

- Personally identifiable information (name, email, phone, account ID, payment details)
- Usage telemetry, crash reports, analytics, A/B-testing pings
- Window titles, process names, or file paths (these are only read locally — see above)
- Behavioural tracking of any kind

There are no third-party analytics or crash-reporting SDKs in the build (no Sentry, no Google Analytics, no Firebase, no Mixpanel, etc.). gmux does not bundle any HTTP client library and does not open HTTP connections of its own.

## Network activity

The only network-adjacent calls gmux makes go through Microsoft's built-in `Windows.Services.Store` WinRT API:

- **Licence check** — roughly every 6 hours in the background, gmux asks Windows whether your Microsoft account owns the Premium add-on. The request only includes the add-on's product ID; gmux sends no personal data.
- **Purchase flow** — when you click *Buy* in the settings window, gmux asks Windows to show the standard Store purchase dialog. Microsoft handles the transaction; gmux never sees your payment information.

These calls are routed by Windows itself.

## Diagnostics export (manual, opt-in)

The tray menu has an *Export diagnostics* item that writes a `.zip` to a location you choose. The zip contains:

- `system_info.txt` — gmux version, OS version, current Windows username
- Your `config.toml` (the path to your log directory is redacted to `<redacted>` before export)
- An engine-state snapshot — current bindings, hotkey state, MRU window list
- The most recent rotated log files

This zip is **not uploaded automatically** anywhere. It is created only when you click the menu item, written to a location you pick, and intended for you to attach to a bug report at your discretion. We never see it unless you decide to send it to us.

## Microsoft Store capabilities

The MSIX manifest declares only one capability:

- `runFullTrust` — required because gmux is a desktop app that uses Win32 hotkeys, window enumeration, and a tray icon

No `internetClient`, camera, microphone, location, contacts, file-system broker, or other sensitive capabilities are requested. The `Windows.Services.Store` licensing calls are routed by the Windows system service and do not require the app itself to hold network capability.

## Third parties

- **Microsoft Store** processes purchases and tells Windows whether you own the Premium add-on. Microsoft's own privacy statement applies to those interactions: <https://privacy.microsoft.com>

No other third-party services are involved.

## Children

gmux is a productivity tool aimed at adult professional users. It does not knowingly collect data from anyone, including children under 13.

## Changes to this policy

If we change what gmux does with data, we will update this page and refresh the *Last updated* date above. Material changes will also be called out in the app's release notes.

## Contact

Questions, bug reports or privacy concerns: please open an issue at <https://github.com/gzhajimi/gmux/issues>.
