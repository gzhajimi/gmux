---
title: gmux — declarative workspace switcher
---

# gmux

声明式工作场景切换器。一行配置 + 一个快捷键，把多显示器布局秒切到任意场景。键盘党友好、`git` 可托管、不破坏现有窗口。

A declarative workspace switcher for Windows. One config file + one hotkey to flip multi-monitor layouts in 0.3s. Keyboard-first, git-friendly, non-destructive.

**Language**: [中文](#中文) · [English](#english)

---

## 中文

### 它解决什么问题

每天你的窗口是这样的混乱：
- 左屏 IDE、右屏 Slack、笔记本 Chrome —— 切到「会议模式」要手动拖 7 个窗口
- 加班回家把笔记本带回，单屏多窗口堆叠，找窗口靠 Alt+Tab 翻三次
- PowerToys Workspaces 倒是能切场景，但**它会把已开着的应用关掉再重启**

gmux 的回答：写一份 TOML 文件描述「哪些应用、放在哪块屏的哪个位置」，剩下的交给引擎：

```
按 Ctrl+M 然后按一个数字键 → 0.3 秒切到对应场景。
```

应用已经在跑就**直接复用**（移动到新位置），没启动的才启动。像换电视频道，不是重启电脑。

### 安装

| 渠道 | 适合 | 链接 |
|---|---|---|
| **Microsoft Store** | 普通用户、想要付费高级功能、自动更新 | _审核中，上架后这里更新_ |
| **GitHub Release**（NSIS 安装包） | 企业封闭网、中国区无 Store、想 git 托管 config | _首版 release 准备中_ |

**系统要求**：Windows 10 build 17763 (1809) 及以上，64 位。

### 5 分钟入门

#### 1. 装好后第一次启动

托盘出现 gmux 图标 → 右键 → **设置**，自动打开配置窗口。第一次启动 gmux 会在 `%APPDATA%\gmux\config.toml` 写一份最简模板。

#### 2. 写第一份配置

最简单的形态——「按 Ctrl+M + 1 把记事本满屏到主屏」：

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
profile = "home"
categories = { main = "full" }
regions = [
  { on = "main", slot = "full", app = "notepad" },
]
```

托盘 → **重新加载配置** → 按 `Ctrl+M` 然后 `1` → 记事本启动并满屏到主屏。再按一次 → 复用同一个记事本窗口（不重新启动）。

#### 3. 加第二个绑定（四象限布局）

```toml
[apps.calc]
aumid = "Microsoft.WindowsCalculator_8wekyb3d8bbwe!App"

[apps.charmap]
exe = "C:/Windows/System32/charmap.exe"

[bindings.2]
description = "四象限"
[[bindings.2.variants]]
profile = "solo"
categories = { main = "quad_h" }
regions = [
  { on = "main", slot = "tl", app = "notepad" },
  { on = "main", slot = "tr", app = "calc" },
  { on = "main", slot = "bl", app = "charmap" },
  # tr 故意空着, 引擎不会强制塞东西
]
```

`Ctrl+M + 2` → 三个应用按四象限分布（左上、右上、左下，右下空着）。

### 核心概念

#### Binding（绑定）

`[bindings.<key>]` —— 一个快捷键触发的目标布局。`<key>` 可以是数字 `0-9` 或字母 `a-z`。一个 binding 可以有多个 **variant**，引擎按当前显示器配置自动选最匹配的。

#### Variant（变体）

`[[bindings.<key>.variants]]` —— 同一 binding 在不同物理显示器配置下的不同布局。比如：
- 家里 3 屏 → 左屏 IDE、中屏 Chrome、右屏笔记
- 路上单屏 → 单屏只放 IDE

声明两个 variant，引擎按当前显示器自动挑一个。

#### Profile（显示器配置）

`[display_profiles.<name>]` —— 一组物理显示器的声明，按 EDID 物理 ID 识别（同型号多屏靠序列号区分）。

```toml
[display_profiles.home]
display = [
  { name = "main" },
  { name = "vertical" },
]
```

#### Display（显示器）

`[[display]]` —— 单台物理屏的注册：

```toml
[[display]]
id = "__PRIMARY__"            # 主屏占位 ID, 也可以是 "AOC2702@SN-A1B2C3"
name = "main"                  # 在 profile 里引用的名字
orientation = "landscape"      # landscape | portrait
```

#### Category × Slot（类别 × 槽位）

布局几何用「类别」+「槽位」二级查找。`quad_h` 类别有 4 个槽位 `tl / tr / bl / br`，`halves_h` 有 `left / right`，等等。

**25 个内置类别**：
- 共享：`full`
- 横屏（12）：`halves_h` `left_wide` `right_wide` `thirds_h` `center_wide` `l_left_eq_h` `l_right_eq_h` `l_left_h` `l_right_h` `quad_h` `hex_h` `oct_h`
- 竖屏（12）：`halves_v` `top_tall` `bot_tall` `thirds_v` `center_tall` `l_top_eq_v` `l_bot_eq_v` `l_top_v` `l_bot_v` `quad_v` `hex_v` `oct_v`

每个类别的具体槽位名 / 几何比例见配置窗口的「类别预览」面板。不允许自定义类别——保持类别集稳定可让 GUI 编辑器永远「认得」你的配置。

#### App（应用）

`[apps.<key>]` —— 应用的启动方式 + 窗口匹配规则。

```toml
[apps.chrome]
exe = "C:/Program Files/Google/Chrome/Application/chrome.exe"

[apps.calc]
aumid = "Microsoft.WindowsCalculator_8wekyb3d8bbwe!App"   # UWP / Store 应用

[apps.weixin]
exe = "C:/Program Files/Tencent/WeChat/WeChat.exe"
[apps.weixin.window_identity]
class = "WeChatMainWndForPC"           # 排除登录窗口、子对话框等
```

引擎自动按 exe basename 或 AUMID 识别窗口。歧义场景（终端、Explorer 多窗口、企业微信内嵌邮件）通过 `[apps.<key>.window_identity]` 显式指定 `class` / `title_contains` / `process` / `aumid`。

### 快捷键速查（默认值，可在 `[actions]` 重新映射）

#### 触发 binding

| 键 | 动作 |
|---|---|
| `Ctrl+M` `<digit-or-letter>` | 触发对应 `[bindings.<key>]`（如 `Ctrl+M 1` / `Ctrl+M a`） |

#### 布局动作（在 `[actions]` 配置）

| 键 | 动作 | 名字 |
|---|---|---|
| `Ctrl+M n` | 切到下一个布局 | `next_layout` |
| `Ctrl+M p` | 切到上一个布局 | `prev_layout` |
| `Ctrl+M l` | 跳到上次的布局 | `last_layout` |
| `Ctrl+M r` | 重新执行当前布局（恢复被手动拖动的窗口） | `restore` |
| `Ctrl+M w` | 列出所有布局（OSD） | `list_layouts` |
| `Ctrl+M space` | 显示桌面（最小化所有） | `show_desktop` |

#### Region 动作

| 键 | 动作 | 名字 |
|---|---|---|
| `Ctrl+M q` `<digit>` | 选区域 N（屏幕浮层显示编号） | `show_regions` |
| `Ctrl+M f` | 在已选区域里循环切到该 app 的下一个窗口 | `cycle_region` |
| `Ctrl+M z` | 已选区域临时全屏 / 还原 | `fullscreen` |
| `Ctrl+M g` | 仅聚焦已选区域（不动窗口） | `focus_region` |
| `Ctrl+M c` | 区域内开第二个窗口（分屏） | `split_region` |
| `Ctrl+M x` | 关闭已选区域的当前窗口 | `close_window` |

#### 显示器动作

| 键 | 动作 |
|---|---|
| `Ctrl+M m` `<digit><digit>` | 交换两块显示器（如 `Ctrl+M m 1 2`） |

### 免费版 vs Premium

**免费版**永久免费、无广告、无遥测、无账号要求：
- 最多 9 个 binding
- 1 个 display profile
- 每个 variant 单显示器（不能跨屏布局）

**Premium**（一次买断，永久授权）：
- 无限 binding
- 无限 profile（家 / 办公室 / 路上分别声明）
- 跨多显示器 variant

**30 天免费试用**：第一次触达需要 Premium 的功能时（如建第 10 个 binding / 给 variant 加第二屏 region），弹对话框选「开始试用」/「立即购买」/「继续免费用」。试用一生一次，**不需要登录账号**。

### 常见问题

#### 配置改完按热键没反应？

- 托盘 → **重新加载配置**（或重启 gmux）
- 看托盘图标：变成红警告图标 = prefix 热键被别的应用占了；右键托盘 → 重试 prefix
- 托盘 → 设置 → 看是否有 TOML 解析错误，会显示行号 + 期望格式

#### 多显示器拔插后变体不切换？

引擎只在启动 / 重载时计算 variant 匹配，热插拔目前要重载。Phase 12+ 计划支持监听 `WM_DISPLAYCHANGE` 自动重算。

#### 同型号两块屏怎么区分？

EDID 序列号自动区分。`%APPDATA%\gmux\config.toml` 里 `[[display]]` 的 `id` 字段格式 `<model>@SN-<serial>`，由 GUI 检测显示器时自动写入。

#### 我的应用 gmux 找不到窗口怎么办？

显式指定 `[apps.<key>.window_identity]`：

```toml
[apps.terminal]
exe = "C:/Users/<you>/AppData/Local/Microsoft/WindowsApps/wt.exe"
[apps.terminal.window_identity]
class = "CASCADIA_HOSTING_WINDOW_CLASS"     # Windows Terminal 的类名
title_contains = "PowerShell"                # 进一步缩窄到具体 tab
```

托盘 → 设置 → 应用 → 「+ 添加应用」会引导你抓正在跑的窗口元数据，省去手写。

#### 配置能跨机器同步吗？

`%APPDATA%\gmux\config.toml` 是一份纯 TOML，可以丢进 `git` / Dropbox / OneDrive。

注意：`display_profiles` 里的 EDID 序列号是**特定机器的**，换机器要么重新跑「检测显示器」让 GUI 写入新机的 ID，要么提前给两台机器的对应屏取一致的 `name` 让 binding 配置可移植。

#### 我能完全用键盘配置吗？

可以。`config.toml` 直接编辑就生效（重载即用）。GUI 是 alternative，不是必经路径。

### 隐私

gmux **不收集任何个人数据**。详情见：
- [中文隐私政策](privacy-zh.md)
- [English Privacy Policy](privacy-en.md)

### 反馈 / 报告问题

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) —— 功能建议、bug 报告、配置疑问统一走 Issues，方便其他用户搜到同样问题的解答。

### 设计哲学

gmux 不是平铺窗口管理器（**不是** yabai / komorebi），不是 Snap Zones（**不是** MaxTo），不是录制式工作流（**不是** Keyboard Maestro）。它专注一件事：**让场景切换成为你一天里摩擦力最低的动作**。

- 声明式：你写「我要什么」，引擎处理「怎么做」
- 非破坏性：不关已开应用，不重启窗口
- Prefix 键：腾出全部全局快捷键空间
- 极致轻量：< 10 MB 安装包，< 50 MB 运行内存
- 100% 本地：无云、无账号、无订阅

### License

gmux 二进制按 Microsoft Store / GitHub Release 各自的服务条款分发。本仓库（文档站）按 [MIT 协议](LICENSE) 公开（如未附 LICENSE 文件，默认 MIT）。

---

## English

### What it solves

Your window state is chaos by 3pm:
- IDE on left, Slack on right, Chrome on the laptop — switching to "meeting mode" means dragging seven windows
- Coming home with the laptop, single-monitor stack — finding a window means three Alt+Tab passes
- PowerToys Workspaces *can* switch scenes but **closes and relaunches your apps** to do it

gmux's answer: write a TOML file describing "which apps, on which monitor, in which position." The engine handles the rest:

```
Press Ctrl+M then a digit → snap to the matching scene in 0.3s.
```

Apps that are already running are **moved**, not restarted. Apps that aren't running get launched. Like changing TV channels, not rebooting.

### Install

| Channel | Best for | Link |
|---|---|---|
| **Microsoft Store** | Regular users, premium features, auto-update | _Pending review; URL TBA_ |
| **GitHub Release** (NSIS installer) | Enterprise networks, China, git-tracked configs | _First release pending_ |

**System requirements**: Windows 10 build 17763 (1809) or later, 64-bit.

### 5-minute quickstart

#### 1. First launch

A tray icon appears → right-click → **Settings** opens the config window. On first launch gmux writes a minimal template to `%APPDATA%\gmux\config.toml`.

#### 2. Your first scene

Simplest possible form — "press Ctrl+M + 1, full-screen Notepad on the main display":

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
regions = [
  { on = "main", slot = "full", app = "notepad" },
]
```

Tray → **Reload Config** → press `Ctrl+M` then `1` → Notepad launches and fills the main display. Press it again → the same Notepad window is reused (no relaunch).

#### 3. A four-quadrant scene

```toml
[apps.calc]
aumid = "Microsoft.WindowsCalculator_8wekyb3d8bbwe!App"

[apps.charmap]
exe = "C:/Windows/System32/charmap.exe"

[bindings.2]
description = "Quadrants"
[[bindings.2.variants]]
profile = "solo"
categories = { main = "quad_h" }
regions = [
  { on = "main", slot = "tl", app = "notepad" },
  { on = "main", slot = "tr", app = "calc" },
  { on = "main", slot = "bl", app = "charmap" },
  # br left empty on purpose; the engine doesn't auto-fill it
]
```

`Ctrl+M + 2` → three apps in the upper-left, upper-right, and lower-right quadrants; lower-right stays empty.

### Core concepts

#### Binding

`[bindings.<key>]` — a hotkey-triggered target layout. `<key>` is a digit `0-9` or letter `a-z`. A binding can have multiple **variants**; the engine picks the one matching your current monitor setup.

#### Variant

`[[bindings.<key>.variants]]` — different layouts for the same binding under different physical monitor configurations. Example:
- 3 monitors at home → IDE / browser / notes split across them
- 1 monitor on the road → IDE only, full-screen

Declare two variants, the engine auto-selects.

#### Profile

`[display_profiles.<name>]` — a named set of physical monitors, identified by EDID hardware IDs (same-model monitors disambiguated by serial number).

```toml
[display_profiles.home]
display = [
  { name = "main" },
  { name = "vertical" },
]
```

#### Display

`[[display]]` — registration of one physical monitor:

```toml
[[display]]
id = "__PRIMARY__"           # placeholder for primary; or "AOC2702@SN-A1B2C3"
name = "main"                 # name referenced by profiles
orientation = "landscape"     # landscape | portrait
```

#### Category × Slot

Layout geometry uses a "category" + "slot" two-level lookup. `quad_h` defines slots `tl / tr / bl / br`, `halves_h` defines `left / right`, etc.

**25 built-in categories**:
- Shared: `full`
- Landscape (12): `halves_h` `left_wide` `right_wide` `thirds_h` `center_wide` `l_left_eq_h` `l_right_eq_h` `l_left_h` `l_right_h` `quad_h` `hex_h` `oct_h`
- Portrait (12): `halves_v` `top_tall` `bot_tall` `thirds_v` `center_tall` `l_top_eq_v` `l_bot_eq_v` `l_top_v` `l_bot_v` `quad_v` `hex_v` `oct_v`

Slot names and pixel ratios per category are documented in the GUI's "Category preview" panel. Custom categories aren't supported on purpose — a stable category set keeps the GUI editor universally compatible.

#### App

`[apps.<key>]` — how to launch an app + how to identify its windows.

```toml
[apps.chrome]
exe = "C:/Program Files/Google/Chrome/Application/chrome.exe"

[apps.calc]
aumid = "Microsoft.WindowsCalculator_8wekyb3d8bbwe!App"   # UWP / Store apps

[apps.terminal]
exe = "C:/Users/<you>/AppData/Local/Microsoft/WindowsApps/wt.exe"
[apps.terminal.window_identity]
class = "CASCADIA_HOSTING_WINDOW_CLASS"
title_contains = "PowerShell"
```

By default, gmux identifies windows by exe basename or AUMID. For ambiguous cases (terminals, Explorer's multiple window classes, embedded mail in WeCom), narrow it down with `[apps.<key>.window_identity]`.

### Hotkey cheatsheet (defaults; remappable in `[actions]`)

#### Trigger a binding

| Keys | Action |
|---|---|
| `Ctrl+M` `<digit-or-letter>` | Fire `[bindings.<key>]` (e.g. `Ctrl+M 1`, `Ctrl+M a`) |

#### Layout actions (config in `[actions]`)

| Keys | Action | Name |
|---|---|---|
| `Ctrl+M n` | Next layout | `next_layout` |
| `Ctrl+M p` | Previous layout | `prev_layout` |
| `Ctrl+M l` | Jump to last layout | `last_layout` |
| `Ctrl+M r` | Re-apply current layout (recover after manual drag) | `restore` |
| `Ctrl+M w` | List all layouts (OSD) | `list_layouts` |
| `Ctrl+M space` | Show desktop (minimize all) | `show_desktop` |

#### Region actions

| Keys | Action | Name |
|---|---|---|
| `Ctrl+M q` `<digit>` | Select region N (overlay shows region numbers) | `show_regions` |
| `Ctrl+M f` | Cycle to next window of the app in the selected region | `cycle_region` |
| `Ctrl+M z` | Toggle temporary fullscreen on selected region | `fullscreen` |
| `Ctrl+M g` | Focus selected region (don't move windows) | `focus_region` |
| `Ctrl+M c` | Open a second window in the selected region (split) | `split_region` |
| `Ctrl+M x` | Close the current window in selected region | `close_window` |

#### Display actions

| Keys | Action |
|---|---|
| `Ctrl+M m` `<digit><digit>` | Swap two displays (e.g. `Ctrl+M m 1 2`) |

### Free vs. Premium

**Free**, forever, no ads, no telemetry, no account required:
- Up to 9 bindings
- One display profile
- Single-display variants only

**Premium** (one-time purchase, lifetime license):
- Unlimited bindings
- Unlimited profiles (home / office / travel)
- Multi-display variants

**30-day free trial**: the first time you hit a Premium feature (10th binding, second-display region in a variant, etc.), a dialog offers "Start trial" / "Buy now" / "Stay free." One trial per machine, **no signup required**.

### FAQ

#### Hotkey doesn't fire after editing the config

- Tray → **Reload Config**, or restart gmux
- Look at the tray icon: red warning badge = your prefix is held by another app; right-click tray → Retry prefix
- Tray → Settings will show TOML parse errors with line numbers + expected format

#### Variant doesn't switch when I (un)plug a monitor

The engine evaluates variants only on launch / reload. Hot-plug currently requires a manual reload. Phase 12+ will listen to `WM_DISPLAYCHANGE`.

#### How are two same-model monitors distinguished?

By EDID serial number. Each `[[display]]` entry's `id` field looks like `<model>@SN-<serial>` and is auto-written by the GUI's "Detect monitors" wizard.

#### gmux can't find my app's window — what now?

Specify `[apps.<key>.window_identity]` explicitly. The GUI's "Add app" wizard captures live window metadata so you don't have to write it by hand.

#### Can I sync my config across machines?

`%APPDATA%\gmux\config.toml` is plain TOML — drop it in git / Dropbox / OneDrive.

Caveat: EDID serial numbers in `display_profiles` are machine-specific. Switching machines means either re-running "Detect monitors" on the new box, or pre-naming both machines' equivalent monitors with the same `name` so binding configs stay portable.

#### Is the GUI mandatory?

No. `config.toml` works directly (reload to apply). The GUI is an alternative, not a requirement.

### Privacy

gmux **does not collect any personal data**. See:
- [中文隐私政策](privacy-zh.md)
- [English Privacy Policy](privacy-en.md)

### Feedback / bug reports

[GitHub Issues](https://github.com/gzhajimi/gmux/issues) — feature requests, bug reports, and config questions all go through Issues so other users can find the same answers.

### Design philosophy

gmux is **not** a tiling WM (yabai / komorebi), **not** snap zones (MaxTo), **not** a workflow recorder (Keyboard Maestro). It does one thing:

> Make scene switches the lowest-friction action in your day.

- **Declarative**: describe the destination, not the steps
- **Non-destructive**: running apps move, not relaunch
- **Prefix key**: leaves the global hotkey namespace untouched
- **Lightweight**: < 10 MB installer, < 50 MB idle RAM
- **100% local**: no cloud, no account, no subscription

### License

The gmux binary is distributed under the respective Microsoft Store / GitHub Release terms. This repository (documentation site) is published under the [MIT License](LICENSE) (defaults to MIT if no `LICENSE` file is present).
