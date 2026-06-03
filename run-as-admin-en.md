# Running as Administrator

> **Language** · [中文](run-as-admin-zh.md) · **English** · [日本語](run-as-admin-ja.md)

← [Back to home](index.md)

---

## Two Switches

Under **Settings → System → Launch** you'll find two checkboxes:

| Switch | What it does |
|--------|-------------|
| **Run as administrator on launch** | Every time you open gmux, a UAC prompt appears. After you confirm, gmux runs with admin privileges |
| **Run as administrator on autostart** | When gmux starts at login, it runs with admin privileges (via Windows Task Scheduler — no UAC prompt) |

The two are fully independent — turning one on or off never changes the other. By default, "Run as administrator on autostart" is enabled (it goes through Task Scheduler, so no UAC prompt) while "Run as administrator on launch" is disabled.

## Why Admin Privileges?

Windows enforces **UIPI** (User Interface Privilege Isolation): a lower-privilege process cannot manipulate windows owned by a higher-privilege process.

In practice, if gmux runs at normal privilege and one of your apps runs elevated (e.g. Registry Editor, certain anti-cheat games, some dev tools), gmux cannot:

- **Move or resize** that window (`SetWindowPos` is blocked by UIPI)
- **Activate or focus** that window
- **Launch** apps that require UAC elevation

When this happens, gmux shows a gold-bordered warning on the affected region: "xxx runs as administrator — gmux can't manage its window".

## What Running as Admin Solves

When gmux itself runs elevated, its privilege level is at least as high as any app, so UIPI restrictions disappear. This means:

- All windows can be moved, resized, and activated normally
- Apps requiring elevation can be launched without issues
- No more privilege-related warnings

## Trade-offs

- **UAC prompt** on every manual launch (autostart via Task Scheduler does not prompt)
- **Security**: admin privileges give gmux (and any child processes it spawns) broader system access. gmux de-elevates child processes automatically, but this is not guaranteed in all scenarios
- If none of your apps require admin privileges, there's no reason to enable this
