---
title: gmux 隐私政策
lang: zh-CN
---

# gmux 隐私政策

最后更新：2026-04-30

## 简短版

gmux 完全在你本机运行，不收集个人数据、不发遥测、不联系任何第三方服务器。唯一会离开你电脑的数据是向 Microsoft Store 查询授权（仅含产品 ID，不含个人信息），且这个调用走的是 Windows 自带的 `Windows.Services.Store` 接口。

## 适用范围

本政策适用于通过 Microsoft Store 分发的 gmux 桌面应用。

- 包名：`D4681314.gmux`
- 发布者：哈基米（CN=727E2FF6-A413-46ED-8027-74D82A3CD038）

## gmux 在本机读取的内容（仅在内存中）

为了把运行中的窗口排到你定义的版型里，gmux 通过标准 Win32 接口读取本机的窗口元数据：

- 窗口标题、窗口类名
- 进程名、可执行文件路径、命令行
- 前台窗口切换事件（用于维护"最近使用窗口"列表，每个应用最多保留 32 条）

以上数据只在内存里处理，**绝不会离开你的电脑**。

## gmux 在本机保存的内容

| 文件 / 位置 | 用途 |
| --- | --- |
| `%APPDATA%\gmux\config.toml` | 你的场景与绑定配置 |
| `%APPDATA%\gmux\license.toml` | Microsoft Store 授权状态缓存（仅 Store 版） |
| `%APPDATA%\gmux\logs\gmux.log.*` | 滚动日志（错误、生命周期事件；滚动周期和保留份数可在设置里调整） |
| Microsoft Store Startup Task `gmux_autostart`（Store 版）—— 在 MSIX 清单里声明，**默认关闭** | 开机自启。由你打开；想关掉随时可以在 Windows 的"启动应用"里关闭。 |
| 命名互斥量 `Global\gmux_mutex_v1` | 防止 gmux 双开，本身不存数据。 |

这些文件都留在你电脑上，不会上传。卸载 gmux 时只会删除程序本体；如果你也想清除配置和日志，可手动删除 `%APPDATA%\gmux\` 目录。

## gmux 不会收集 / 不会发送

- 个人身份信息（姓名、邮箱、电话、账号 ID、支付信息）
- 使用统计 / 遥测 / 崩溃报告 / A-B 测试上报
- 窗口标题、进程名、文件路径（这些只在本机读取，见上一节）
- 任何形式的用户行为追踪

构建产物中不包含任何第三方分析或崩溃上报 SDK（无 Sentry、无 Google Analytics、无 Firebase、无 Mixpanel 等）。gmux 不打包任何 HTTP 客户端库，也不会自行发起 HTTP 连接。

## 网络请求

gmux 唯一与网络相关的调用全部通过 Windows 自带的 `Windows.Services.Store` WinRT 接口：

- **授权状态检查** —— 大约每 6 小时一次，在后台请 Windows 查询当前 Microsoft 账号是否已购买 Premium add-on。请求中只含 add-on 的产品 ID，gmux 本身不发任何个人数据。
- **购买流程** —— 你在设置里点「购买」时，gmux 请 Windows 弹出系统级的 Store 购买对话框。交易由 Microsoft 处理，gmux 接触不到你的支付信息。

这些调用由 Windows 自己路由。

## 诊断导出（手动、需你主动触发）

托盘菜单中的「导出诊断信息」会把一个 zip 写到你选择的位置，里面包含：

- `system_info.txt`（gmux 版本号、操作系统版本、当前 Windows 用户名）
- 你的 `config.toml`（导出前会把日志目录路径替换成 `<redacted>`）
- 引擎当前状态快照（绑定、热键状态、最近使用窗口列表）
- 最近的几份滚动日志

这个 zip **不会自动上传**到任何地方，只在你点击菜单时生成、写到你指定的位置，方便你提交 bug 时附带。除非你主动把它发给我们，我们看不到它。

## Microsoft Store 权限申请

MSIX 清单中仅声明一项能力：

- `runFullTrust` —— gmux 是桌面应用，需要使用 Win32 全局热键、窗口枚举和托盘图标

不申请 `internetClient`、摄像头、麦克风、位置、联系人、文件系统代理等任何敏感能力。`Windows.Services.Store` 授权调用由 Windows 系统服务路由，不需要应用本身具备网络能力。

## 第三方服务

- **Microsoft Store** 负责处理购买交易，并告诉 Windows 你是否拥有 Premium add-on。这部分交互适用 Microsoft 自己的隐私声明：<https://privacy.microsoft.com>

不涉及任何其他第三方服务。

## 儿童

gmux 是面向成年专业用户的生产力工具，不会有意收集任何人（包括 13 岁以下儿童）的数据。

## 政策变更

如果 gmux 处理数据的方式发生变化，我们会更新本页面并刷新顶部「最后更新」日期。重大变更也会在应用的发布日志中说明。

## 联系方式

问题、bug 反馈或隐私相关疑问：请到 GitHub Issues 留言 <https://github.com/gzhajimi/gmux/issues>。
