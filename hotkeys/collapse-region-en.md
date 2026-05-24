---
title: prefix+d — Collapse region (minimize window)
---

# prefix+d — Collapse the selected region, minimizing its window

> **Language** · [中文](collapse-region-zh.md) · **English** · [日本語](collapse-region-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+d` reclaims the selected region's tile and **minimizes** its window to the taskbar. Unlike [`prefix+x`](close-window-en.md), it **doesn't close** the window — the app keeps running and the window is fully recoverable.

> Default chord: `d` · Config key: `collapse_region` · Change in Settings → Hotkeys → Region operations

---

## Trigger flow

```
1. (optional) press prefix+q N    select a region; if you skip, region 0 is the default
2.            press prefix+d      that region's current window is minimized (SW_MINIMIZE)
```

> One-shot: `prefix+q` → `d` → `digit` selects that region and runs this action immediately (see [prefix+q](show-regions-en.md)).

---

## Minimize semantics

Uses the Win32 `SW_MINIMIZE` show command:

- **The window stays alive**: it goes to the taskbar; the app keeps running and nothing is closed.
- **Not vetoable**: there is no "Save?" handshake like `WM_CLOSE` — minimizing always succeeds and never risks unsaved data.
- **Fully recoverable**: click the taskbar button to restore, or re-trigger the binding (see below).

---

## What happens after collapsing

- The temporary fullscreen flag for that region is cleared.
- The tile is evicted from the layout immediately.
- If the region was produced by a split, it is **reclaimed** — the sibling region expands to take the space back.
- The engine does **not** auto-place a new window into the now-gone tile.
- To bring the window back: click it in the taskbar; or re-trigger the binding (`prefix` + its key) — the engine re-discovers the window via MRU and un-minimizes it back into place. `prefix+r` re-places the whole layout (also un-minimizes); `prefix+f` promotes the next window of that app.

---

## Difference from prefix+x (close)

Both `prefix+d` and [`prefix+x`](close-window-en.md) evict the region's tile and reclaim split sub-regions. The difference is what happens to the **window**:

| | `prefix+d` collapse_region | `prefix+x` close_window |
|---|---|---|
| Win32 action | `SW_MINIMIZE` | `WM_CLOSE` |
| Window fate | minimized, stays alive | closed (the app decides) |
| "Save?" prompt / app can veto | no | yes |
| Data-loss risk | none | possible (unsaved docs) |
| Reversible | yes — restore from taskbar / re-trigger | no undo |
| Region tile eviction | yes | yes |
| Split sub-region reclaim | yes | yes |

> Rule of thumb: use `prefix+d` to *stash* a window you'll want back; use `prefix+x` to actually *close* it.

---

## Rejection conditions

| Situation | Behavior |
|---|---|
| No layout currently available | Silent |
| selected_region has no window in that slot | Silent; also clears the stale fullscreen flag for that region |
| The window is already dead | Silent |

---

## I collapsed the wrong region

Nothing was destroyed — collapsing is non-destructive.

- The window isn't gone: find it in the taskbar and click to restore.
- Or re-trigger the binding to re-place and un-minimize it.
- Unlike [`prefix+x`](close-window-en.md), there is no app close flow and no "Save?" dialog to worry about.
