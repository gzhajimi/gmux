---
title: prefix+x — 关闭区域窗口
---

# prefix+x — 关闭选中区域的当前窗口

> **语言** · **中文** · [English](close-window-en.md) · [日本語](close-window-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+x` 关闭选中 region 当前那一个窗口。**不重启应用**，只关那一个窗口。

> 默认 chord：`x` · 配置项：`close_window` · 在「设置 → 快捷键 → 区域操作」改

---

## 触发流程

```
1. （可选）按  prefix+q N    选中 region；不选默认 region 0
2.        按  prefix+x       该 region 的当前窗口收到 WM_CLOSE
```

---

## 关闭语义

走标准 Win32 `WM_CLOSE` 消息：

- **异步**：发出后立即返回，应用自己决定何时关闭。
- **应用可拦截**：未保存的文档应用（Office / VSCode / 记事本）会弹「保存？」对话框；用户点取消的话窗口不会关。
- **应用可忽略**：极少数应用（某些游戏 / 守护进程）干脆不响应 `WM_CLOSE`，按 `prefix+x` 看上去就「没反应」。

> 如果你想强行结束应用进程（不走 `WM_CLOSE`），请用任务管理器 / `Stop-Process`，gmux 不提供这种破坏性能力。

---

## 关闭后会发生什么

- 该 region 的临时全屏标记被清掉。
- 引擎不会自动给 region 摆下一个窗口。
- 想看「下一个 Chrome 窗口怎么自动顶上」：用 `prefix+f`。
- 想看「整个布局重新摆一遍」：用 `prefix+r`。
- region 本身不消失（除非 split 出来的 region 在某些清理路径下回归）。

---

## 拒绝条件

| 情况 | 行为 |
|---|---|
| 当前没有可用布局 | 静默 |
| selected_region 对应位置没有窗口 | 静默；同时清掉该 region 的 stale 全屏标记 |
| 窗口已死 | 静默 |

---

## 误关后怎么办

`prefix+x` 走的是应用自己的关闭流程，**没有撤销**。

- 如果应用支持「最近关闭」（浏览器 Ctrl+Shift+T、IDE 的 reopen closed），用应用原生功能恢复。
- 如果是文档应用，弹了「保存？」对话框时点取消即可避免误关。
