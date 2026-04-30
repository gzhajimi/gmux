---
title: prefix+p — Previous layout
---

# prefix+p — Switch to the previous layout

> **Language** · [中文](prev-layout-zh.md) · **English** · [日本語](prev-layout-ja.md)

[← Back to Settings Help](../settings-help-en.md)

`prefix+p` is the reverse of [`prefix+n`](next-layout-en.md): it walks one step backward through the matching layout list.

> Default chord: `p` · Config key: `prev_layout` · Change in Settings → Hotkeys → Layout navigation

---

## Behavior

- The list scope, ordering, and skip rules are exactly the same as `prefix+n`.
- Wraps from the head back to the tail (reverse cycle).
- Can be pressed in rapid succession.
- Same silent rejection conditions.

---

## Relationship to next_layout

`prefix+n` and `prefix+p` go together: if you overshoot with `n`, press `p` to back up one.

If you only ever toggle between 2 layouts, [`prefix+l`](last-layout-en.md) is more ergonomic than `n/p` — you don't need to count steps.

---

See [the prefix+n doc](next-layout-en.md) for details.
