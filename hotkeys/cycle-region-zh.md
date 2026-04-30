---
title: prefix+f — 循环切换区域窗口
---

# prefix+f — 在区域里轮换 app 的多个窗口

> **语言** · **中文** · [English](cycle-region-en.md) · [日本語](cycle-region-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+f` 把选中 region 当前应用的**下一个窗口**拉到这个 region 来。

> 默认 chord：`f` · 配置项：`cycle_region` · 在「设置 → 快捷键 → 区域操作」改

---

## 典型场景

布局把 region 0 摆成 Chrome，但你开了 5 个 Chrome 窗口。第一次触发布局把第 1 个 Chrome 摆进 region 0；想看第 2、第 3 个时按 `prefix+f` 一直轮，每按一次切到下一个，循环回到头。

---

## 触发流程

```
1. （可选）按  prefix+q N    选中 region N；不选默认 region 0
2.        按  prefix+f       下一个窗口被移到该 region 并激活
3.        再按  prefix+f      下下一个 …
```

---

## 候选窗口怎么挑

引擎按以下顺序：

1. 取该 region 当前 app 的**所有运行中窗口**。
2. 排除当前布局**其它 region 已经占用**的窗口（避免把别的 region 的窗口偷走）。
3. 把剩下的按 MRU（最近使用）排序作为循环列表。
4. 第一次按 `f` 时，从列表里挑「不是当前那一个」的最新一个。
5. 后续按 `f` 在列表里向后走，到末尾回到开头。

> 如果列表只有 1 个候选窗口（包括「就是当前这个」），按 `f` **静默无操作**。

---

## 循环会话的有效期

按 `f` 之间的间隔受**循环超时**控制（默认 `cycle_timeout_ms = 3000 ms`）：

- 3 秒内连按 `f` 视为同一次循环会话，cursor 持续向后走。
- 超过 3 秒再按 `f` → 重新开一个会话，从「不是当前那一个」开始。

会话还会被这些事件**强制清空**：

- 切换布局（`prefix+<key>` / `prefix+n/p/l` / `prefix+w`）。
- 候选列表里某个窗口被关闭。
- `prefix+r`（restore）。
- `prefix+m` swap 完成。
- 配置重载。

---

## 全屏 / 原生最大化的处理

- 当前 region 处于 `prefix+z` 临时全屏：轮换出来的下一个窗口仍然以**全屏尺寸**摆进去（保持全屏视觉）。
- 当前布局对该 region 设置了 `is_native_maximize`（原生窗口最大化标志）：轮换的窗口走 `maximize_native` 而不是 `move_window`，避免某些应用（Office / 浏览器）在自定义大小下渲染异常。

---

## 选中状态记住

`prefix+f` 用 selected_region 作为目标。一旦你按 `prefix+q N` 选过 region，后续连按 `f / z / g / c / x` 全都对 region N 生效，**不需要每次都重新选**。

只有切到别的绑定再切回时，selected_region 才可能变化（每个绑定独立记忆）。

---

## 拒绝条件

| 情况 | 行为 |
|---|---|
| 当前没有可用布局 | 静默 |
| selected_region 在当前布局里不存在 | 静默 |
| 该 region 的 app 没在 `[apps]` 里注册 | 报错（理论上不应该发生） |
| 候选列表少于 2 个窗口 | 静默 |
| cursor 指向的窗口已死 | 清掉会话，本次按键无效 |

---

## 时序参数

| 参数 | 默认 | 设置位置 |
|---|---|---|
| 同会话最长间隔 | `3000 ms` | 快捷键 → 高级 → 循环超时（`cycle_timeout_ms`） |
