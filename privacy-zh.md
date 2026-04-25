---
title: gmux 隐私政策
lang: zh-CN
---

# gmux 隐私政策

最后更新：2026-04-25

## 简短版

gmux 不收集你的任何个人数据。它在你电脑本地运行，不联网（除了向 Microsoft Store 查询授权状态）。

## 详细说明

### 我们不收集

- 个人身份信息（姓名、邮箱、电话）
- 使用统计 / 遥测 / 崩溃报告
- 你的窗口标题、应用名、文件路径
- 任何形式的用户行为追踪

### 我们存储在你本机的数据

- `%APPDATA%\gmux\config.toml` — 你写的场景配置文件
- `%APPDATA%\gmux\license.toml` — 授权状态缓存（仅 Store 版有）

这些文件不会被上传到任何服务器。卸载 gmux 时你可选择是否保留这些文件。

### 网络请求

gmux 仅向 Microsoft Store 发起以下网络请求：

- 启动时查询当前账号是否已购买 Premium add-on
- 用户点「立即购买」时打开 Store 系统购买对话框
- 每 6 小时后台静默刷新一次授权状态

这些请求由 Windows 自带的 `Windows.Services.Store` API 发起，gmux 不直接访问任何第三方服务器。

### 第三方服务

- **Microsoft Store**：负责购买交易、授权管理。Microsoft 的隐私政策见 <https://privacy.microsoft.com>

### 联系方式

问题或反馈请到 GitHub Issues：<https://github.com/gzhajimi/gmux/issues>
