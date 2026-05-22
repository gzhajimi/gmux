---
title: gmux — 声明式工作场景切换器
---

中文 · [English](index.md) · [日本語](ja.md) · [GitHub](https://github.com/gzhajimi/gmux)

# gmux

**Windows 上的声明式工作场景切换器。**
在图形设置界面里描述好你的多显示器布局，按一个热键，瞬间就位。

## 工作流程

1. **描述** —— 打开 gmux 设置界面，点选你的布局：显示器、应用、区域。全程图形化，不用手写配置文件。
2. **按 `Ctrl+M` + 一个键** —— 你的 chord（`1`、`2`、`q` …）。
3. **窗口就位** —— 已经开着的应用直接**复用**（移动到新位置），不关、不重启。

## 获取 gmux

**gmux 现已上架 Microsoft Store** —— 自动更新、30 天 Premium 试用、无需账号。

[**前往 Microsoft Store 获取 →**](https://apps.microsoft.com/detail/9MWP3799MCLH)

**macOS** 版本正在准备中。[在 GitHub 给个 Star](https://github.com/gzhajimi/gmux)，上架时第一时间收到通知。

**系统要求：** Windows 10 build 17763 (1809) 及以上，64 位。

## 30 秒看懂

在设置界面里点几下，就配好一个把记事本铺满主屏的 binding：选显示器、挑布局、把应用放进区域。

按 `Ctrl+M` 然后 `1` → 记事本满屏到主屏。再按一次 → 复用同一个窗口（不重启）。

**配置可托管、可同步。** 你在界面里配好的一切，gmux 会存成一份声明式 TOML。你不用手写它，但它是一份纯文本配置 —— 可以备份、丢进 git / Dropbox / OneDrive 跨机器同步，换机后直接复用：

```toml
[bindings.1]
description = "记事本满屏"
[[bindings.1.variants]]
profile = "solo"
categories = { main = "full" }
regions = [{ on = "main", slot = "maximize", app = "notepad" }]
```

## 为什么是 gmux

- **图形配置** —— 显示器检测、布局、应用、快捷键全在设置界面里点选，无需手写配置文件。
- **声明式** —— 你描述「我要什么」，引擎处理「怎么做」。
- **非破坏性** —— 已开应用是被移动，不是被重启。
- **Prefix 键** —— 腾出全部全局快捷键空间。
- **极致轻量** —— < 10 MB 安装包，< 50 MB 运行内存。
- **100% 本地** —— 无云、无账号、无订阅、无遥测。

gmux **不是**平铺窗口管理器（不是 yabai / komorebi），**不是** Snap Zones（不是 MaxTo），**不是**录制式工作流（不是 Keyboard Maestro）。它专注一件事 —— 让场景切换成为你一天里摩擦力最低的动作。

## 免费版 vs Premium

**免费版** —— 永久免费、无广告、无遥测、无账号要求：最多 9 个 binding、1 个 display profile、单显示器 variant。

**Premium**（一次买断、永久授权） —— 无限 binding、无限 profile（家 / 办公室 / 路上）、跨多显示器 variant。

**30 天免费试用** —— 第一次触达需要 Premium 的功能时弹对话框，**不需登录账号**，一生一次。

## 文档

- [使用指南](settings-help-zh.md) —— 五个核心概念、所有内置快捷键、设置页逐项说明
- [常见问题](faq-zh.md) —— 热键不响应、多屏识别、跨机同步等
- [隐私政策](privacy-zh.md)

## 反馈

功能建议、bug 报告、配置疑问统一走 [GitHub Issues](https://github.com/gzhajimi/gmux/issues)，方便其他用户搜到同样问题的解答。

---

gmux 程序遵循 Microsoft Store 的相应许可条款。本文档站以 [MIT License](LICENSE) 发布。
