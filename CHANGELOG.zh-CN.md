# 更新日志

面向用户的 Rayterm 发布说明。逐 commit 的完整历史在源码仓库里；本文件只
记录每次更新值得留意的内容。

每个版本的二进制和签名见
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases)。

English: [CHANGELOG.md](CHANGELOG.md)

---

## v1.0.13 — 2026-05-21

### Shell 终端预置命令

Shell 类终端的配置里新增 **预置命令** 字段：写在里面的命令会在终端起来
后逐行自动执行，省掉每次手动 `nvm use 18`、`source ~/.proxyrc`、
`conda activate xxx` 的步骤。

- 编辑器以弹窗形式承载，不挤占设置对话框的篇幅；主界面只显示一行
  「已配置 N 条」摘要 + 编辑按钮。
- 单条命令失败不会卡死终端：和 RUN（exec）终端不同，预置命令是当作
  用户敲键盘那样发送的，某行报错只是输出错误信息，shell 仍然能继续
  正常使用。
- 支持 `#` 开头的注释行，方便分组备注。

### VibeCoding / RUN 环境变量

VibeCoding 与 RUN（exec）终端的配置新增 **环境变量** 编辑器，使用键值
对的形式填写：

- 本地终端：变量直接传给子进程，cmd / PowerShell / bash 都通用。
- WSL / SSH 终端：自动转成 `export KEY='VALUE'; ...; <你的命令>` 注入
  到远端 bash 启动命令前。用户无需关心目标系统的不同语法，rayterm
  会按 OS 自动生成兼容形式。
- 弹窗 UI 同预置命令，主界面只占一行摘要。

### 修复

- **误触 Ctrl+R / F5 清空所有终端 UI**：原先 webview 直接走浏览器
  reload，会把整个 React 树清掉，后台 PTY 进程虽然还在跑但前端再也
  接不上，看起来像「所有终端都消失了」。现在生产构建里拦截了
  `F5`、`Ctrl/Cmd+R`、`Ctrl+Shift+R`、`Ctrl+F5` 这些 reload 快捷键；
  开发模式下保留 HMR fallback 不受影响。
- **PowerShell 终端的预置命令出现在 `>>` 续行提示后不执行**：PTY 里
  Enter 实际上是 CR (`\r`)，PSReadLine 把 `\n` 当作缓冲区内的软换行。
  把行分隔符改成 `\r` 之后 PowerShell 也能正常按行提交，bash / zsh
  的 readline 都接受。

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

v1.0.13 之前各版本的发布说明，请见
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases) 上
对应版本页。要点：

- **v1.0.12** — 工程管理器与星标工程；AI 扫描 Claude 计费提醒；
  Windows 下 npm `.cmd` shim 路径解析修复。
- **v1.0.11** — 启动时自动检查更新横幅（可关闭）。
- **v1.0.10** — 退出确认；切 tab 自动聚焦终端；探测 Trae / Visual
  Studio / Android Studio / Xcode 作为编辑器。
- **v1.0.9**  — 日志监视器终端（本地 / WSL / SSH `tail -F`）。
- **v1.0.8**  — trzsz 文件传输；终端标签分离到独立窗口；字体 UI 打磨；
  配色方案导入；编程连字符支持。
- **v1.0.7**  — tmux 持久会话；工程树与终端配置打磨。
- **v1.0.6**  — 应用内自动更新；macOS 启动 PATH 修复。
