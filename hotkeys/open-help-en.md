---
title: prefix+? — Open help
---

# prefix+? — Open the Help / cheatsheet window

> **Language** · [中文](open-help-zh.md) · **English** · [日本語](open-help-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+?` opens the Settings window and jumps to the Help page (cheatsheet).

> Default chord: `?` · Config key: `open_help` · Change in Settings → Hotkeys → Layout navigation

---

## Implementation detail of the default key

Users see `?`, but internally the engine treats it as **equivalent to the `/` key**:

- No need to hold Shift.
- On Chinese / English / Japanese keyboards alike it uses the `/` key (typically next to `.`).
- Assigning `?` to another action on the Hotkeys page won't conflict, because the engine does shift-OEM alias normalization.

---

## Behavior

After `prefix+?`:

- If the Settings window is already open → jump to the Help tab and bring the window to the foreground.
- If the Settings window is not open → open it and land on the Help page.
- No visual side effects / does not change the current layout / does not change selected_region.

---

## What the Help page shows

The Help page is a dynamically generated cheatsheet:

- **Current prefix** (reflects edits to prefix immediately).
- **All bindings**: each shown as `prefix → <key>` with its description.
- **All built-in actions**: each shown as `prefix → <chord>` with the action's description.
- Documentation and support links: online docs, report an issue, About gmux.

Edits to bindings / chords automatically refresh this page through the `config-reloaded` event.

---

## Relationship to F1

When the Settings window is focused, `F1` is equivalent to `prefix+?`.

Exception: F1 doesn't respond on the **About** and **Open-source licenses** pages (to avoid interrupting reading). Use the sidebar to reach Help from there.

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| Callback not installed at startup (only happens in standalone CLI / test builds) | Silent no-op |

Under a normal Tauri shell build it always succeeds in opening.
