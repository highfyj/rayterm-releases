# 更新日志

面向用户的 Rayterm 发布说明。逐 commit 的完整历史在源码仓库里；本文件只
记录每次更新值得留意的内容。

每个版本的二进制和签名见
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases)。

English: [CHANGELOG.md](CHANGELOG.md)

---

## v1.0.12 — 2026-05-18

### 工程管理器与星标工程

左侧侧边栏的工程列表现在**只显示加了星标的工程**，常用工程不再被一长串
列表淹没。

- 右键点击侧边栏的任一工程，弹出菜单：**重命名**（内联编辑）、**取消
  星标**、**AI 扫描**、**删除**。原本悬停才显示的「AI 扫描 / 删除」
  小图标改放到菜单里，避免误触；「在编辑器中打开 / 编辑设置」两个
  图标仍保留在行内。
- 侧边栏底部新增**全部工程**入口，常驻显示工程总数。点击打开**工程
  管理器**对话框。
- 工程管理器左侧是全部工程列表（支持搜索、星标切换、`+` 新建）；右侧
  是详情卡片，列出根目录、连接目标、Agent、创建时间、终端清单和子目录
  数量。
- 新建工程默认带星标。升级前已有的工程会被视为已加星，不会因为升级而
  从侧边栏消失。

### AI 扫描：Claude 计费提醒

把 AI 扫描 agent 切到 **Claude** 时，会弹出一次性提醒：Anthropic 将于
2026-06-15 把 `claude -p` 迁到独立的 Agent SDK 额度池。可以选择继续用
Claude、一键切到 OpenCode，或永久关闭该提醒。

### 修复

- **Windows: 用 OpenCode / Claude / Codex 做 AI 扫描** 之前会报
  `spawn opencode: program not found`，即便 CLI 确实装好了。原因是 npm
  装的 CLI 在 Windows 下其实是 `.cmd` 包装，Rust 的 `Command::new` 不
  查 `PATHEXT`。现在改用 PATHEXT 感知的查找解析绝对路径，shim 能被
  正确找到。

---

## 历史版本

v1.0.12 之前各版本的发布说明，请见
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases) 上
对应版本页。要点：

- **v1.0.11** — 启动时自动检查更新横幅（可关闭）。
- **v1.0.10** — 退出确认；切 tab 自动聚焦终端；探测 Trae / Visual
  Studio / Android Studio / Xcode 作为编辑器。
- **v1.0.9**  — 日志监视器终端（本地 / WSL / SSH `tail -F`）。
- **v1.0.8**  — trzsz 文件传输；终端标签分离到独立窗口；字体 UI 打磨；
  配色方案导入；编程连字符支持。
- **v1.0.7**  — tmux 持久会话；工程树与终端配置打磨。
- **v1.0.6**  — 应用内自动更新；macOS 启动 PATH 修复。
