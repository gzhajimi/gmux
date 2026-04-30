---
title: prefix+space — 显示桌面
---

# prefix+space — 显示桌面 / 还原

> **语言** · **中文** · [English](show-desktop-en.md) · [日本語](show-desktop-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+space` 切换桌面显示状态：所有窗口最小化 ↔ 还原。

> 默认 chord：`space` · 配置项：`show_desktop` · 在「设置 → 快捷键 → 布局导航」改

---

## 行为

- 第一次按：所有窗口最小化，露出桌面。
- 再按一次：刚才被最小化的窗口全部还原回原位。

等价于系统的 `Win+D`，只是改走 prefix —— 适合不想给 Win 键单独留快捷键的用户。

---

## 触发流程

直接按 `prefix+space`，无需第二键。

---

## 注意

- 这是一个**系统级**的桌面切换，不区分 gmux 摆放的窗口和别的窗口 —— 所有非托盘窗口都被最小化。
- 还原时也是所有窗口一起还原。如果你在最小化期间又手动操作了某些窗口，再按 `prefix+space` 还原可能与你的预期略有出入（这是 Windows 自身的 toggle desktop 行为，gmux 不做特殊处理）。
- 不影响 selected_region / 当前布局 / 全屏标记。

---

## 拒绝条件

无。`prefix+space` 永远会执行（不依赖任何 gmux 状态）。
