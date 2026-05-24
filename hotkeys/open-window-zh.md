---
title: prefix+o — 打开窗口（应用选择器）
---

# prefix+o — 从应用选择器打开窗口，放到目标屏

> **语言** · **中文** · [English](open-window-en.md) · [日本語](open-window-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+o` 打开与 [`prefix+c`](split-region-zh.md) 相同的应用选择器，然后把选中应用的窗口放到**目标屏**（默认为最后一块显示器；可通过下文的 `prefix+m+数字+o` 序列覆盖），按配置的大小居中摆放。和 `prefix+c` 不同，它**不创建也不占用 region**——被放置的窗口不参与 restore、cycle、focus、swap 等布局操作。

> 默认 chord：`o` · 配置项：`open_window` · 尺寸设置：`[hotkey].open_window_percent`（50–100，默认 75）· 在「设置 → 快捷键 → 区域操作」改绑

---

## 触发流程

```
1. 按  prefix+o         应用选择器浮层弹出
2. 输入关键词           按名称过滤应用列表
3. 按 Enter（或点击）   选中应用的窗口被放到目标屏
```

---

## 窗口摆放

窗口被放到**目标屏**，居中：

- **默认目标**：最后一块显示器（系统几何中编号最高的那块屏）。
- **覆盖目标**：执行 `prefix+m` → 屏编号 → `o`（见 [prefix+m — 交换显示器](swap-displays-zh.md)）后，引擎把选中的屏设为粘性目标，此后 `prefix+o` 在该屏上打开。目标在重启、显示器热插拔 / 重新探测、或再次执行 `prefix+m+数字+o` 时清空。
- **尺寸**：由 `[hotkey].open_window_percent`（50–100）控制。`100` = 填满该显示器的工作区；`50` = 工作区的一半。
- **位置**：在目标屏的工作区内居中。
- 不创建 region。该窗口不进入布局，也不被 binding/region 槽位记忆追踪。

---

## 复用优先，找不到再新建

在启动新实例之前，引擎会先找该应用的**空闲窗口**——即当前布局中尚未被放置的窗口：

| 应用窗口状态 | 发生什么 |
|---|---|
| 存在空闲（未放置）窗口 | 该窗口被移到最后一块屏并调整大小 / 居中 |
| 没有空闲窗口，但应用在运行 | 启动新实例，然后放置 |
| 应用未在运行 | 启动应用，然后放置 |

引擎**绝不关闭**现有窗口来满足此动作（非破坏性）。

---

## 不受影响的操作

`prefix+o` **不干扰**布局感知操作：

- `prefix+r`（restore）——不还原或移动已打开的窗口
- `prefix+f`（cycle_region）——不循环到它
- `prefix+g`（focus_region）——不聚焦到它
- `prefix+m`（swap_displays）——不随之移动
- Region 槽位记忆——该窗口不被按 region 追踪

再次按 `prefix+o` 只会重新弹出选择器，之前放置的窗口保持原样。

---

## 与 prefix+c（split_region）的区别

两个动作打开的是同一个应用选择器。区别在于选中应用之后的行为：

| | `prefix+o` open_window | `prefix+c` split_region |
|---|---|---|
| 创建 region | 否 | 是（切分选中 region） |
| 显示器 | 目标屏（默认最后一块屏，可通过 `prefix+m+数字+o` 改变） | 选中 region 所在的屏 |
| 尺寸 | `open_window_percent`（50–100 %） | 由切分布局决定 |
| 参与 restore/cycle/focus | 否 | 是 |
| 影响布局 | 否 | 是 |

---

## 拒绝条件

| 情况 | 行为 |
|---|---|
| 选择器被关闭（Esc / 没有选中） | 静默；不移动或启动任何窗口 |
| 应用没有 `exe` 定义且未在运行 | 静默；选择器正常关闭 |

---

## 配置

在 `config.toml` 里修改 chord 或放置尺寸：

```toml
[hotkey]
open_window_percent = 80   # 50–100；默认 75（100 = 填满工作区）

[actions]
open_window = "o"          # 默认值；设为 "" 可禁用
```

也可在「设置 → 快捷键 → Advanced → Open size」拖动滑块调整。
