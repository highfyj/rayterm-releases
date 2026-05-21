# Changelog

User-facing release notes for Rayterm. The full per-commit history lives in
the source repo; this file highlights what's worth noticing when you update.

For binaries and signatures of every release, see
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases).

中文版本：[CHANGELOG.zh-CN.md](CHANGELOG.zh-CN.md)

---

## v1.0.13 — 2026-05-21

### Preset commands for shell terminals

Shell terminal configs gained a **Preset commands** field: lines you put
there run automatically as soon as the shell starts, so you don't have
to retype `nvm use 18`, `source ~/.proxyrc`, or `conda activate xxx`
every time.

- The editor lives in a popup so the main config dialog stays compact —
  you just see a one-line summary ("N commands configured") and an
  Edit button.
- A failing line does **not** lock the terminal. Unlike RUN (exec)
  terminals, preset commands are typed in as if the user hit Enter, so
  an error prints to the terminal and the prompt comes back ready for
  the next command.
- Lines starting with `#` are treated as comments.

### Environment variables for VibeCoding / RUN terminals

VibeCoding and RUN (exec) terminal configs gained an **Environment
variables** key/value editor:

- Local terminals: variables are passed directly through the spawned
  process env — works the same on cmd / PowerShell / bash.
- WSL / SSH terminals: rayterm prepends `export KEY='VALUE'; ...;
  <your command>` to the remote bash launch line, so users don't have
  to learn the per-OS syntax. We always emit POSIX `export` for the
  remote side regardless of the local OS.
- Same popup UX as preset commands — only a one-line summary in the
  main dialog.

### Fixes

- **Accidental Ctrl+R / F5 wiping the terminal UI**: previously, the
  webview's default reload handler ran on any of those shortcuts,
  blowing away the entire React tree. The backend PTY processes kept
  running but the frontend had no way to reattach, which looked like
  "all my terminals just disappeared". Production builds now intercept
  `F5`, `Ctrl/Cmd+R`, `Ctrl+Shift+R`, and `Ctrl+F5` before the webview
  reacts; dev mode keeps the HMR-fallback reload working.
- **PowerShell preset commands stuck at the `>>` continuation prompt**:
  PTYs deliver Enter as CR (`\r`), and PSReadLine treats `\n` as a soft
  newline inside the input buffer rather than submitting the line. The
  preset writer now uses `\r` as the line separator, which submits each
  line on every shell — bash and zsh readline accept it too.

---

## v1.0.12 — 2026-05-18

### Project Manager & Starred Projects

The left sidebar's project list now shows **only starred projects**, so the
panel stays focused on what you actually work on day-to-day.

- Right-click any project in the sidebar for a context menu: **Rename**
  (inline), **Unstar**, **AI Scan**, **Delete**. The previous hover icons
  for AI scan and delete moved into this menu to avoid mis-clicks; the
  "Open in editor" and "Edit settings" icons remain on the row.
- A new **All projects** entry pinned to the bottom of the sidebar shows
  the total project count and opens the **Project Manager** dialog.
- Project Manager has a left list (every project, with search, star
  toggle, and a `+` to create new ones) and a right detail pane (root
  path, target, agent, created date, terminal list, folder summary).
- New projects are starred by default. Projects created before this
  release are treated as starred so nothing disappears after the upgrade.

### AI Scan: Claude billing notice

Selecting **Claude** as the AI scan agent now shows a one-time notice
about Anthropic's 2026-06-15 change that moves `claude -p` to a separate
Agent SDK credit pool. You can continue with Claude, switch to OpenCode
in one click, or dismiss the notice permanently.

### Fixes

- **Windows: AI scan with OpenCode / Claude / Codex** previously failed
  with `spawn opencode: program not found` even when the CLI was clearly
  installed. The root cause was that npm-installed CLIs ship as `.cmd`
  shims on Windows, and Rust's raw `Command::new` doesn't consult
  `PATHEXT`. Scans now resolve the CLI through a PATHEXT-aware lookup,
  so the shim is found correctly.

---

## Earlier releases

For release notes prior to v1.0.13, see the per-release pages on
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases).
Highlights:

- **v1.0.12** — Project Manager + starred projects; Claude billing
  notice for AI scan; Windows fix for resolving npm `.cmd` shims.
- **v1.0.11** — Startup auto-update banner (dismissible).
- **v1.0.10** — Exit confirmation; tab switch auto-focuses the terminal;
  Trae / Visual Studio / Android Studio / Xcode detected as editors.
- **v1.0.9**  — Log monitor terminal kind (local / WSL / SSH `tail -F`).
- **v1.0.8**  — trzsz file transfer; pop-out tabs into separate windows;
  font UI polish; color-scheme import; programming ligatures.
- **v1.0.7**  — Persistent tmux sessions; project tree & terminal config
  polish.
- **v1.0.6**  — In-app updater; macOS login-shell PATH fix.
