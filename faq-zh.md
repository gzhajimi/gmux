---
title: gmux 常见问题
---

# gmux 常见问题

> **语言** · **中文** · [English](faq-en.md)

← [回到首页](index.md)

---

### 配置改完按热键没反应？

- 托盘 → **重新加载配置**（或重启 gmux）。
- 看托盘图标：变成红警告图标 = prefix 热键被别的应用占了；右键托盘 → **重试 prefix**。
- 托盘 → 设置 → 看是否有 TOML 解析错误，会显示行号 + 期望格式。

### 多显示器拔插后变体不切换？

gmux 会监听显示器变化（`WM_DISPLAYCHANGE`，去抖约 500 ms）：插拔显示器后自动重新枚举显示器并重新编译绑定，下次触发就会选中匹配新布局的 variant，无需手动重载。变化前已摆好的窗口不会自动重排，再按一次该绑定或 `prefix+r` 即可重新就位。

### 同型号两块屏怎么区分？

EDID 序列号自动区分。`%APPDATA%\gmux\config.toml` 里 `[[display]]` 的 `id` 字段格式 `<model>@SN-<serial>`，由 GUI 检测显示器时自动写入。

详细概念见 [使用指南 → 显示器](settings-help-zh.md#1-显示器display)。

### 我的应用 gmux 找不到窗口怎么办？

在 `[apps.<key>.match]` 下显式写匹配规则：

```toml
[apps.terminal]
launch.exe = "C:/Users/<you>/AppData/Local/Microsoft/WindowsApps/wt.exe"
match.hard.class = "CASCADIA_HOSTING_WINDOW_CLASS"   # Windows Terminal 的窗口类名

[[apps.terminal.match.soft]]
kind = "title_contains"     # 向某个具体 tab 倾斜
pattern = "PowerShell"
weight = 50
```

`match.hard.*` 字段（`process` / `process_path` / `class` / `aumid`）是精确硬条件；`[[apps.<key>.match.soft]]` 是带权重的软提示（`kind` = `title_contains` / `title_regex` / `class_prefix` / `process_prefix`，`weight` 取值 −100…100）。

托盘 → 设置 → 应用 → 「+ 添加应用」会引导你抓正在跑的窗口元数据，省去手写。

### 配置能跨机器同步吗？

`%APPDATA%\gmux\config.toml` 是一份纯 TOML，可以丢进 `git` / Dropbox / OneDrive。

注意：`display_profiles` 里的 EDID 序列号是**特定机器的**，换机器要么重新跑「检测显示器」让 GUI 写入新机的 ID，要么提前给两台机器的对应屏取一致的 `name` 让 binding 配置可移植。

### 我能完全用键盘配置吗？

可以。`config.toml` 直接编辑就生效（重载即用）。GUI 是 alternative，不是必经路径。

### 还没找到答案？

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) —— 功能建议、bug 报告、配置疑问统一走 Issues。
