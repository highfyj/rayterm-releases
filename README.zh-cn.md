# Rayterm

[English](README.md)

Rayterm 是一个面向开发者、运维和 AI 编码工作流的跨平台终端工作台。它把项目、终端、远程连接、脚本、文件传输和 AI CLI 工具集中到一个桌面应用里，让常用工程环境可以像 IDE 工作区一样被保存、切换和复用。

![Rayterm 主界面](assets/screenshots/rayterm-main.png)

## 适合谁使用

- 经常在多个项目、目录、服务器、WSL 环境之间切换的开发者。
- 需要同时管理本地 Shell、SSH、串口、构建脚本和文件传输的运维/嵌入式开发场景。
- 正在使用 Claude、Codex、OpenCode 等 AI CLI 工具，希望把 AI 终端纳入项目工作台的人。
- 想要一个轻量、跨平台、工程化的终端管理工具，而不是只开一堆独立终端窗口。

## 核心能力

- **项目工作台**：把本地目录、WSL 或 SSH 目标组织成项目，项目下可保存常用终端、脚本、AI 工具和文件入口。
- **多标签与分屏终端**：每个标签页可以独立分屏，适合同时查看日志、运行构建、打开 AI agent 或连接远程服务器。
- **本地 / WSL / SSH**：支持常见本地 shell、WSL 发行版和 SSH 连接，适合 Windows、macOS、Linux 混合开发环境。
- **AI CLI 集成**：可以把 Claude、Codex、OpenCode 等命令行工具作为项目终端固定下来，一键进入对应项目上下文。
- **脚本与运行任务**：为项目保存构建、测试、启动、部署等常用命令，减少重复输入。
- **文件与传输**：支持本地文件管理与远程 SFTP 场景，便于在终端旁边处理文件。
- **串口终端**：支持串口设备连接、参数配置和自动重连，适合设备调试。
- **主题与体验**：暗色界面、字体设置、快捷键、锁屏、语言切换、配置导入导出和缓存清理。

## 下载

请从 GitHub Releases 下载对应平台的安装包：

[下载最新版 Rayterm](https://github.com/highfyj/rayterm-releases/releases/latest)

通常会提供以下平台构建产物：

- Windows x64
- macOS Apple Silicon / Intel
- Linux x64

如果你已经安装过支持自动更新的版本，发布正式版本后应用会按更新配置检查新版本。

## 快速上手

1. 安装并启动 Rayterm。
2. 在左侧项目区添加或选择一个项目。
3. 为项目创建常用终端，例如 Shell、构建命令、SSH、Claude Agent 或 OpenCode。
4. 使用顶部标签页在 Home、Terminal、Run、AI 工具之间切换。
5. 需要并行工作时，在终端面板里使用分屏，把日志、构建、调试和 AI 对话放在同一个工作区。

## 常见使用场景

### 项目开发

把项目目录固定到侧边栏，为它保存 `dev`、`build`、`test`、`tauri build` 等命令。下次打开项目时，不需要重新输入路径和命令。

### AI 编码

为项目创建 Claude、Codex 或 OpenCode 终端，让 AI CLI 默认运行在项目目录中。这样可以一边看代码/日志，一边让 AI 工具执行分析、修改或构建命令。

### 远程运维

为常用服务器保存 SSH 终端和远程文件入口。需要查看日志、传输文件或执行部署命令时，可以从项目工作台直接进入。

### 设备调试

创建串口终端并保存 baud rate、parity、stop bits、flow control、DTR/RTS 和自动重连等参数。设备重连或应用重启后，可以快速恢复调试环境。

## 平台说明

- Windows 推荐 Windows 10/11，并确保 WebView2 可用。
- macOS 首次运行可能需要在系统安全设置中允许打开。
- Linux 版本依赖系统图形环境和 WebKitGTK 运行库，不同发行版的桌面环境可能存在差异。

## 数据与隐私

Rayterm 的项目配置和应用设置保存在本机用户目录下。SSH、AI CLI、数据库 CLI 等外部工具由用户自己的环境提供，Rayterm 主要负责启动、组织和展示这些工作流。

使用 AI CLI 或远程连接时，请注意不要把敏感项目、密钥、服务器信息或本地配置提交到公开仓库或不可信服务。

## 反馈

如果遇到安装、启动、更新或平台兼容问题，请在 release 仓库提交 issue：

[提交问题](https://github.com/highfyj/rayterm-releases/issues)
