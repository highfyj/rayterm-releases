# Changelog

User-facing release notes for Rayterm. The full per-commit history lives in
the source repo; this file highlights what's worth noticing when you update.

For binaries and signatures of every release, see
[GitHub Releases](https://github.com/highfyj/rayterm-releases/releases).

中文版本：[CHANGELOG.zh-CN.md](CHANGELOG.zh-CN.md)

---

## v1.0.18 — 2026-06-12

### Fixes

- **Dropping a file no longer wipes the UI.** Dragging an HTML, image,
  or audio file into the Rayterm window used to make the webview
  navigate straight to that file — every terminal vanished behind the
  file's content until you restarted. The window now rejects unclaimed
  drops (you'll see a "not allowed" cursor); pages inside the browser
  pane still receive drag-and-drop as before, and future in-app drop
  targets (e.g. upload-by-drop) are unaffected.
- **macOS: Chinese IME swallowing characters / double spaces.**
  (macOS only; Windows was never affected.) These are upstream
  xterm.js defects on WKWebView, now fully worked around at the app
  layer:
  - With a Chinese IME, the first character typed after toggling to
    English via Shift — or a half-width symbol typed directly — is no
    longer swallowed.
  - Space no longer produces two spaces while an IME is active
    (either 中/英 sub-mode).

---

## v1.0.17 — 2026-06-03

### Tools page + one-click programming-font installer

Rayterm gains a **Tools** page — open it from the new wrench icon in the
top-right title bar. It's a hub for built-in utilities, and the first one
is a **font installer**.

- **Install programming fonts in one click.** Pick from JetBrains Mono,
  Fira Code, Cascadia Code, Hack, Source Code Pro, and IBM Plex Mono.
  Rayterm fetches the font's latest GitHub release, streams the download
  with a live progress bar, unzips it, and installs the `.ttf`/`.otf`
  files for you — then the font is ready to choose in the terminal font
  picker.
- **User or system scope.** Install just for your account (no prompt) or
  system-wide for every user. System installs request elevation the native
  way on each platform — UAC on Windows, an authorization prompt on macOS,
  `pkexec` on Linux.
- Each font shows an **installed badge**, a per-font progress bar while it
  downloads, and the path it was installed to. Fully localized (English /
  中文).

---

## v1.0.16 — 2026-06-01

### Highlights

**AI scan now matches your shell.** When you run an AI scan to generate
terminal suggestions, Rayterm now tells the agent which OS and shell the
commands will actually run in — Windows (PowerShell), macOS (zsh), Linux,
or WSL (bash) — so the suggested `shell` and `script` entries use the
right paths, quoting, and built-ins instead of defaulting to Linux/bash.
A Windows project no longer gets handed bash one-liners it can't run.

**Quick link to all releases.** Settings → About → Updates gains a
**View all releases on GitHub** link — the manual fallback for when the
in-app updater can't run, such as sandboxed installs or corporate
networks that block the updater endpoint.

### Fixes

- **Codex AI agent** — `codex exec` no longer refuses to start in a
  non-Git folder (`--skip-git-repo-check`), and now runs read-only
  (`--sandbox read-only`), which clears the sandbox permission errors
  some hosts — Windows especially — hit during a scan.

---

## v1.0.15 — 2026-05-31

### Save & restore workspaces

You can now snapshot a set of open terminal tabs — split layouts and all —
as a named **workspace**, and bring the whole set back in one gesture.
Workspaces are scoped per project.

- **Save**: the new save icon in the sidebar's *Terminals* header opens a
  dialog listing every open tab. Pick one, several, or all of them, name
  the set, and it's stored with the project. Split-pane layouts are
  preserved exactly.
- **Open**: saved workspaces show in a list under the project's terminal
  tree. Double-click one to open it — a dialog lets you choose **Reopen**
  (close the current tabs and switch to the workspace) or **Open new**
  (add the workspace's tabs alongside what's already open), and previews
  which tabs each choice affects. This list is separate from the terminal
  tree, so it never clashes with the double-click-to-open-terminal gesture.
- **Restore on startup**: a new **Settings → Terminal → Restore last
  workspace on startup** toggle. When on, Rayterm remembers every open tab
  (with splits) on exit and reopens them automatically next launch —
  ad-hoc split shells included.

---

## v1.0.14 — 2026-05-25

### Configurable sidebar double-click + jump to opened terminals

The sidebar's terminal list gets a small but heavily-requested workflow
knob. Previously, double-clicking a terminal config always spawned a
brand-new tab — convenient when you want a fresh shell, annoying when
you've already got that terminal open and just want to focus it.

- **Settings → Terminal → Double-click opens a new terminal** is the
  new toggle. On (default) preserves the legacy behavior. Off makes
  double-click cycle through every already-opened instance of that
  config; if none are open it falls back to a fresh spawn so the
  gesture is never a no-op.
- The cycle order follows the tab strip, then in-tree split order, so
  hitting the same row repeatedly hops predictably across windows.
- The terminal row's right-click menu now lists both **New** and
  **Jump to opened** unconditionally, so either action is one click
  away regardless of how the toggle is set.

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

- **v1.0.13** — Preset commands for shell terminals; environment
  variables for VibeCoding / RUN terminals; intercept accidental
  Ctrl+R / F5 reloads; PowerShell preset commands fix.
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
