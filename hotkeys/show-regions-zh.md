---
title: prefix+q — 选中区域
---

# prefix+q — 显示并选中 region

> **语言** · **中文** · [English](show-regions-en.md) · [日本語](show-regions-ja.md)

[← 返回设置页帮助](../settings-help-zh.md)

`prefix+q` 是所有「区域操作」（`prefix+f / z / g / c / x`）的前置步骤：先把 region 编号画到屏上，按数字选中一个，后续区域操作就对它生效。

> 默认 chord：`q` · 配置项：`show_regions` · 在「设置 → 快捷键 → 区域操作」改

---

## 区域（region）是什么

当前布局的每个 region = **某块屏的某个槽位**（左上 / 右半 / 中部条等）。OSD 弹出时会在每个 region 中央用大数字打出编号，编号从 0 开始。

> 如果当前还没触发任何布局，`prefix+q` 会静默丢弃，因为没东西可选。

---

## 三种用法

### 用法 1：单位数选区（最常用）

```
按  prefix
按  q              ← 屏上每个 region 显示编号 0 1 2 3 …
按  3              ← 选中编号 3 的 region，OSD 闪一下后关闭
```

按完数字后：

- 选中的 region 在 OSD 上**闪烁高亮**确认。
- OSD 关闭。
- 选中状态记入「当前绑定的 selected_region」，被以下动作直接使用：
  - `prefix+f` 在该 region 里轮换 app 多窗口
  - `prefix+z` 该 region 临时全屏
  - `prefix+g` 把焦点跳到该 region 的窗口
  - `prefix+c` 在该 region 上加一个新应用（split）
  - `prefix+x` 关闭该 region 的当前窗口

### 用法 2：两位数选区（region 编号 ≥ 10）

如果当前布局有超过 10 个 region（多屏 oct 类别能到 16 个甚至更多），用第二个 q 切换到「多位输入模式」：

```
按  prefix
按  q              ← OSD 显示编号
按  q              ← 进入多位累积模式（OSD 不变）
按  1
按  2              ← 缓冲区累积到 "12"
按  Enter          ← 提交，选中 region 12
```

多位模式细节：

- 每按一位数字都会**重置**输入超时（默认 `region_input_timeout_ms = 1000 ms`）。
- 超时后**自动按 Enter**：把已经累积的内容当成编号提交（如缓冲是 `1`，提交 region 1；缓冲为空，相当于取消）。
- 按 `Esc` 取消，按 `Enter` 立即提交，按其它任意键取消并把这个键透传。

> 如果你只有 ≤ 10 个 region，永远不需要进入多位模式 —— 直接单位数即可。

### 用法 3：取消

`prefix+q` 之后取消的方式：

- 按 `Esc`：取消，OSD 关闭。
- 按任意非数字、非 q、非 Esc 的键：取消，OSD 关闭，**该按键透传给焦点窗口**。
- 等待总超时（默认 `cycle_timeout_ms = 3000 ms`）：超时取消。
- 切换前台窗口、显示器热插拔、配置重载：自动取消。

---

## 选中状态会被记多久

- **跨 chord 持久**：选中后哪怕过了几小时，再按 `prefix+f / z / g / c / x` 仍然对那个 region 生效。
- **绑定级**：每个绑定有自己独立的 selected_region。切到别的绑定再切回来，原选中保留。
- **被这些事件清除**：
  - 配置重载（`reload`）后所有选中清空。
  - 该 region 的窗口被关闭后，引擎自动 `forget` 该 region 的选中（重新轮询时就回到 region 0）。
  - `prefix+r`（restore）会清掉当前绑定的选中。

---

## 没选中过任何 region 怎么办

`prefix+f / z / g / c / x` 在没有 selected_region 时**自动用 region 0**作为目标。所以最常见的场景是：

- 单 region 布局或全屏布局：直接 `prefix+f` 轮换、`prefix+z` 全屏，从来不需要按 `prefix+q`。
- 多 region 布局：第一次按 `prefix+q N` 选好后，后续操作就连按 `prefix+f` / `prefix+z` 即可，不用每次都重选。

---

## 选了无效编号会怎样

按了一个超过当前布局 region 数的数字，或者编号对应的位置不是叶子（live 布局可能动态合并）：

- OSD 关闭。
- 不修改 selected_region 状态（保持上一次的选中）。
- 后续区域操作仍按原选中工作。

---

## 时序参数

| 参数 | 默认 | 设置位置 |
|---|---|---|
| 单 q 模式总超时 | `3000 ms` | 快捷键 → 高级 → 循环超时（`cycle_timeout_ms`） |
| 多位模式每位数字间隔超时 | `1000 ms` | 快捷键 → 高级 → REGION 编号输入超时（`region_input_timeout_ms`） |
| OSD 内部清理 tick | `1000 ms` | 快捷键 → 高级 → REGION OSD 清理间隔 |
