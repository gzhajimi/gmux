---
title: gmux — declarative workspace switcher
---

# gmux

声明式工作场景切换器 · A declarative workspace switcher for Windows.

**Language**: [中文](#中文) · [English](#english)

---

## 中文

> 把多显示器布局做成 TOML，按 `Ctrl+M` + 一个键 0.3 秒切到任意场景。
> **已开着的应用直接复用** —— 移动到新位置，不关、不重启。

### 30 秒看懂

```toml
[hotkey]
prefix = "Ctrl+M"

[apps.notepad]
exe = "C:/Windows/System32/notepad.exe"

[[display]]
id = "__PRIMARY__"
name = "main"
orientation = "landscape"

[display_profiles.solo]
display = [{ name = "main" }]

[bindings.1]
description = "记事本满屏"
[[bindings.1.variants]]
profile = "solo"
categories = { main = "full" }
regions = [{ on = "main", slot = "full", app = "notepad" }]
```

按 `Ctrl+M` 然后 `1` → 记事本满屏到主屏。再按一次 → 复用同一个窗口（不重启）。

### 安装

| 渠道 | 适合 | 链接 |
|---|---|---|
| **Microsoft Store** | 普通用户、付费高级功能、自动更新 | _审核中，上架后此处更新_ |
| **GitHub Release**（NSIS 安装包） | 企业封闭网、中国区无 Store、想 `git` 托管配置 | _首版 release 准备中_ |

**系统要求**：Windows 10 build 17763 (1809) 及以上，64 位。

### 文档

- 📖 [使用指南](settings-help-zh.md) —— 五个核心概念、所有内置快捷键、设置页逐项说明
- ❓ [常见问题](faq-zh.md) —— 热键不响应、多屏识别、跨机同步等
- 🔒 [隐私政策](privacy-zh.md)

### 免费版 vs Premium

**免费版**永久免费、无广告、无遥测、无账号要求：最多 9 个 binding、1 个 display profile、单显示器 variant。

**Premium**（一次买断、永久授权）：无限 binding、无限 profile（家 / 办公室 / 路上）、跨多显示器 variant。

**30 天免费试用**：第一次触达需要 Premium 的功能时弹对话框，**不需登录账号**，一生一次。

### 反馈

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) —— 功能建议、bug 报告、配置疑问统一走 Issues，方便其他用户搜到同样问题的解答。

### 设计哲学

gmux **不是**平铺窗口管理器（不是 yabai / komorebi），**不是** Snap Zones（不是 MaxTo），**不是**录制式工作流（不是 Keyboard Maestro）。它专注一件事——

> 让场景切换成为你一天里摩擦力最低的动作。

- **声明式**：写「我要什么」，引擎处理「怎么做」
- **非破坏性**：不关已开应用，不重启窗口
- **Prefix 键**：腾出全部全局快捷键空间
- **极致轻量**：< 10 MB 安装包，< 50 MB 运行内存
- **100% 本地**：无云、无账号、无订阅

---

## English

> Describe your multi-monitor layouts in a TOML file, press `Ctrl+M` + a key, and snap to any scene in 0.3s.
> **Apps already running are moved**, not relaunched.

### 30-second tour

```toml
[hotkey]
prefix = "Ctrl+M"

[apps.notepad]
exe = "C:/Windows/System32/notepad.exe"

[[display]]
id = "__PRIMARY__"
name = "main"
orientation = "landscape"

[display_profiles.solo]
display = [{ name = "main" }]

[bindings.1]
description = "Notepad fullscreen"
[[bindings.1.variants]]
profile = "solo"
categories = { main = "full" }
regions = [{ on = "main", slot = "full", app = "notepad" }]
```

Press `Ctrl+M` then `1` → Notepad fills the main display. Press it again → the same window is reused (no relaunch).

### Install

| Channel | Best for | Link |
|---|---|---|
| **Microsoft Store** | Regular users, premium features, auto-update | _Pending review; URL TBA_ |
| **GitHub Release** (NSIS installer) | Enterprise networks, China, git-tracked configs | _First release pending_ |

**System requirements**: Windows 10 build 17763 (1809) or later, 64-bit.

### Documentation

- 📖 [User guide](settings-help-en.md) — five core concepts, every built-in hotkey, settings pages explained
- ❓ [FAQ](faq-en.md) — hotkey not firing, multi-monitor IDs, cross-machine sync
- 🔒 [Privacy policy](privacy-en.md)

### Free vs. Premium

**Free**, forever, no ads, no telemetry, no account required: up to 9 bindings, one display profile, single-display variants.

**Premium** (one-time purchase, lifetime license): unlimited bindings, unlimited profiles (home / office / travel), multi-display variants.

**30-day free trial**: the first time you hit a Premium feature, a dialog offers "Start trial" / "Buy now" / "Stay free." **No signup required**, one trial per machine.

### Feedback

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) — feature requests, bug reports, and config questions all go through Issues so other users can find the same answers.

### Design philosophy

gmux is **not** a tiling WM (yabai / komorebi), **not** snap zones (MaxTo), **not** a workflow recorder (Keyboard Maestro). It does one thing:

> Make scene switches the lowest-friction action in your day.

- **Declarative**: describe the destination, not the steps
- **Non-destructive**: running apps move, not relaunch
- **Prefix key**: leaves the global hotkey namespace untouched
- **Lightweight**: < 10 MB installer, < 50 MB idle RAM
- **100% local**: no cloud, no account, no subscription

---

### License

The gmux binary is distributed under the respective Microsoft Store / GitHub Release terms. This repository (documentation site) is published under the [MIT License](LICENSE) (defaults to MIT if no `LICENSE` file is present).
