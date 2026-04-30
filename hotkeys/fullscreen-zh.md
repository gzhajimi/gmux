---
title: prefix+z — 区域临时全屏
---

# prefix+z — 选中区域临时全屏

> **语言** · **中文** · [English](fullscreen-en.md) · [日本語](fullscreen-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+z` 把选中 region 的窗口放大到**整块屏的工作区**（避开任务栏），再按一次还原。

> 默认 chord：`z` · 配置项：`fullscreen` · 在「设置 → 快捷键 → 区域操作」改

---

## 触发流程

```
1. （可选）按  prefix+q N    选中 region；不选默认 region 0
2.        按  prefix+z       region 当前窗口放大到全屏
3.        再按  prefix+z      还原到原 region 矩形
```

---

## 为什么叫「临时全屏」

它**不修改预案**：

- 状态只活在当前 live 布局里。
- 切到别的绑定再切回 → 全屏标记可能保留（绑定级），也可能因 region 变化失效。
- `prefix+r`（restore）会清掉所有临时全屏，让布局回到 TOML 描述的状态。
- 关闭 region 当前窗口、配置重载等也会清掉对应的全屏标记。

---

## 同屏单全屏规则

为了避免「两个 region 一起全屏」的视觉冲突，引擎对**同一块屏**强制单全屏：

> 如果你在屏 A 的 region 1 已经全屏，按 `prefix+q 2` 选中屏 A 的 region 2 再按 `prefix+z`：
> - 引擎先把 region 1 **降回原矩形**。
> - 然后把 region 2 升为全屏。

跨屏不互相影响。屏 A 的 region 1 全屏 + 屏 B 的 region 3 全屏可以同时存在。

---

## 与 cycle_region (`prefix+f`) 的配合

如果选中 region 当前处于全屏：

- `prefix+f` 轮换出来的下一个窗口**继承全屏状态**，按全屏尺寸摆放。
- 这样可以「全屏轮播」同 app 的多窗口。

---

## 与 close_window (`prefix+x`) 的配合

`prefix+x` 关掉 region 的窗口后，会同时清掉该 region 的全屏标记。下次给该 region 摆新窗口时不会被旧标记影响。

---

## 拒绝条件

| 情况 | 行为 |
|---|---|
| 当前没有可用布局 | 静默 |
| selected_region 不存在或对应位置没有窗口 | 静默；同时清掉该 region 的 stale 全屏标记 |
| 目标窗口已死 | 静默 |

---

## 不是真正的全屏 API

这里的「全屏」是「占满工作区」（避让任务栏），走 `maximize_native`（Win32 SW_MAXIMIZE）。**不是** Win+Shift+Enter 那种独占全屏（exclusive fullscreen），不会触发显示器模式切换、不会让游戏走全屏渲染路径。
