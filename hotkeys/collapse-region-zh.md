---
title: prefix+d — 回收区域（最小化窗口）
---

# prefix+d — 回收选中区域并最小化其窗口

> **语言** · **中文** · [English](collapse-region-en.md) · [日本語](collapse-region-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+d` 回收选中 region 的格子，并把它的窗口**最小化**到任务栏。和 [`prefix+x`](close-window-zh.md) 不同，它**不关闭**窗口——应用继续运行，窗口随时可以找回。

> 默认 chord：`d` · 配置项：`collapse_region` · 在「设置 → 快捷键 → 区域操作」改

---

## 触发流程

```
1. （可选）按  prefix+q N    选中 region；不选默认 region 0
2.        按  prefix+d       该 region 的当前窗口被最小化（SW_MINIMIZE）
```

> 一步到位：`prefix+q` → `d` → `数字`，选中该区并立即执行本操作（详见 [prefix+q](show-regions-zh.md)）。

---

## 最小化语义

走 Win32 `SW_MINIMIZE` 显示命令：

- **窗口不死**：缩到任务栏，应用继续运行，什么都没关。
- **不可拦截**：没有 `WM_CLOSE` 那种「保存？」握手——最小化总会成功，绝不会丢未保存的数据。
- **随时找回**：点任务栏按钮还原，或重新触发那个 binding（见下）。

---

## 回收后会发生什么

- 该 region 的临时全屏标记被清掉。
- 格子立即从布局里移除。
- 如果这个 region 是 split 切出来的，它会被**回收**——兄弟 region 扩张把空间收回去。
- 引擎**不会**自动给这个已消失的格子摆下一个窗口。
- 想把窗口找回来：在任务栏点它还原；或重新触发那个 binding（`prefix` + 它的键）——引擎经 MRU 重新发现该窗口并把它还原摆回原位。`prefix+r` 会重新摆整个布局（同样会还原）；`prefix+f` 会顶上该应用的下一个窗口。

---

## 与 prefix+x（关闭）的区别

`prefix+d` 和 [`prefix+x`](close-window-zh.md) 都会移除 region 的格子、并回收 split 子 region。区别在于对**窗口**做了什么：

| | `prefix+d` collapse_region | `prefix+x` close_window |
|---|---|---|
| Win32 动作 | `SW_MINIMIZE` | `WM_CLOSE` |
| 窗口去向 | 最小化，仍存活 | 关闭（应用自己决定） |
| 「保存？」对话框 / 应用可拦截 | 否 | 是 |
| 丢数据风险 | 无 | 可能（未保存文档） |
| 可逆 | 可——任务栏还原 / 重新触发 | 无撤销 |
| 移除 region 格子 | 是 | 是 |
| 回收 split 子 region | 是 | 是 |

> 经验法则：想「先收起来、回头还要」用 `prefix+d`；想真的「关掉」用 `prefix+x`。

---

## 拒绝条件

| 情况 | 行为 |
|---|---|
| 当前没有可用布局 | 静默 |
| selected_region 对应位置没有窗口 | 静默；同时清掉该 region 的 stale 全屏标记 |
| 窗口已死 | 静默 |

---

## 收错了 region 怎么办

什么都没被销毁——回收是非破坏性的。

- 窗口没丢：在任务栏找到它点一下就还原。
- 或者重新触发那个 binding，把它重新摆回来并还原。
- 和 [`prefix+x`](close-window-zh.md) 不同，这里没有应用的关闭流程、也没有「保存？」对话框要操心。
