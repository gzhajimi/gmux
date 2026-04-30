---
title: prefix+r — 重做当前布局
---

# prefix+r — 重新执行当前布局

> **语言** · **中文** · [English](restore-en.md) · [日本語](restore-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+r` 把当前布局**按 TOML 描述重新执行一遍**：所有 region 重新摆位、临时全屏清除、live split 丢弃。

> 默认 chord：`r` · 配置项：`restore` · 在「设置 → 快捷键 → 布局导航」改

---

## 用得最多的场景

- 手动拖动了某个窗口，想一键归位。
- 试用了 `prefix+c` split 或 `prefix+z` 全屏后想回到「干净」初始状态。
- 某个应用启动慢，第一次触发布局时它还没出现 → 等启动完后 `prefix+r` 把它按预案塞进去。
- 多窗口应用（Chrome 4 个窗口）摆错了实例，`prefix+r` 让 MRU 重新挑一遍。

---

## 触发流程

直接按 `prefix+r`，无需选区或第二键。

---

## 它会清掉什么

- **selected_region**：当前绑定的 region 选中被清空，下一次 `prefix+f / z / g / c / x` 回退到 region 0。
- **临时全屏标记**：所有通过 `prefix+z` 全屏化的 region 回到原矩形。
- **live split**：通过 `prefix+c` 在 live 状态切出来的 region 被丢弃，回到 TOML 描述的初始 region 集合。
- **循环会话**：`prefix+f` 的轮换 cursor 被清空，下次按 f 重新选起点。

---

## 它**不会**清掉什么

- 配置文件本身不变（restore 不写 TOML）。
- 其它绑定的 selected_region / 全屏 / split 状态不动。
- 已经启动的应用进程不动（不重启）。
- 鼠标 / 键盘焦点不动。

---

## 与 `prefix+<key>` 重新触发同一绑定的区别

按 `prefix+1` 触发绑定 1，再按一次 `prefix+1`：

- 等效于 `prefix+r`：重新执行同一布局。
- 但 last_layout 历史不变。

按 `prefix+r`：

- 也是重新执行当前布局。
- last_layout 历史不变。

实际差别极小，习惯哪个用哪个。区别在于 `prefix+r` 不依赖你记得当前是哪个绑定 key。

---

## 拒绝条件

| 情况 | 行为 |
|---|---|
| 还没触发过任何布局（current_layout 为空） | 告警，静默 |
| 当前布局对当前显示器集合无匹配（热插拔后） | 告警，静默；先 `prefix+w` 选一个能匹配的布局 |
