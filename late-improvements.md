# SynthShell — Late Improvements

> Features that are planned but not part of the initial build. These are high-value, high-effort features that will be built after v1.0. Each feature has full spec detail so nothing is lost.

---

## Table of Contents

1. [Natural Language Command Bar](#1-natural-language-command-bar) *(moved to v1.0 — see main requirements)*
2. [Session Recording & Replay](#2-session-recording--replay)
3. [Collaborative Terminal](#3-collaborative-terminal-shared-sessions)
4. [Built-in Log Viewer](#4-built-in-log-viewer)
5. [Terminal-Native API Tester](#5-terminal-native-api-tester)
6. [Voice Commands](#6-voice-commands)
7. [Terminal Games While Waiting](#7-terminal-games-while-waiting)
8. [Plugin Marketplace](#8-plugin-marketplace)
9. [Inline Image Rendering](#9-inline-image-rendering) *(moved from v1.0)*
10. [Smart History Search](#10-smart-history-search) *(moved from v1.0)*
11. [Process Monitor](#11-process-monitor) *(moved from v1.0)*

---

## 1. Natural Language Command Bar

> **Status:** Moved to v1.0 requirements (Feature 16)

---

## 2. Session Recording & Replay

### Description
Record a terminal session (commands + output + timing), replay it later, export as GIF or video. Perfect for creating tutorials, debugging, or showing someone what you did. Asciinema does this standalone — SynthShell has it built in.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| SR-01 | Start/stop recording with hotkey or command | Must |
| SR-02 | Record commands, output, timing, and shell type | Must |
| SR-03 | Replay recorded session with original timing | Must |
| SR-04 | Playback speed control (0.5x, 1x, 2x, 4x) | Must |
| SR-05 | Pause/resume playback | Must |
| SR-06 | Seek to specific timestamp in playback | Should |
| SR-07 | Export as Asciinema v2 format (.cast) | Must |
| SR-08 | Export as GIF (via asciinema-aggif or similar) | Should |
| SR-09 | Export as MP4 video | Could |
| SR-10 | Recording stored in `~/.synthshell/recordings/` | Must |
| SR-11 | Recording metadata (title, date, shell, cwd, duration) | Must |
| SR-12 | List/search past recordings in sidebar | Should |
| SR-13 | Share recording via URL (Asciinema upload integration) | Could |
| SR-14 | Recording does not impact terminal performance (< 2% CPU) | Must |
| SR-15 | Configurable: exclude sensitive output (env vars, API keys) from recording | Must |
| SR-16 | Annotation support — add comments at specific timestamps | Could |

### Acceptance Criteria

- [ ] Recording captures commands and output with accurate timing
- [ ] Replay works with original timing
- [ ] Export to .cast format works
- [ ] Export to GIF works
- [ ] Sensitive data can be excluded from recordings
- [ ] Recording uses < 2% CPU during capture

---

## 3. Collaborative Terminal (Shared Sessions)

### Description
Like tmux but easier. Share a terminal session link, someone joins from their phone or browser, both see the same output. Real-time pair debugging.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| CS-01 | Start shared session with hotkey or command | Must |
| CS-02 | Generate shareable link (URL) for the session | Must |
| CS-03 | Joiner sees terminal output in real time (view-only mode) | Must |
| CS-04 | Joiner can type commands (collaborative mode) | Must |
| CS-05 | Host can toggle view-only vs collaborative per joiner | Must |
| CS-06 | Session password/PIN for access control | Must |
| CS-07 | Max joiners limit (configurable) | Should |
| CS-08 | Joiner cursor indicator (show who is typing) | Should |
| CS-09 | Chat sidebar for text communication during session | Could |
| CS-10 | Session recording (auto-record shared sessions) | Could |
| CS-11 | Works via Tailscale for private network sharing | Must |
| CS-12 | Works via public relay for internet sharing (optional) | Could |
| CS-13 | Join via browser (WebRTC or WebSocket) | Must |
| CS-14 | Join via SynthShell terminal (peer-to-peer) | Should |
| CS-15 | Host can kick/ban joiners | Must |
| CS-16 | Session ends when host disconnects | Must |
| CS-17 | Encrypted transport (E2E) | Must |

### Acceptance Criteria

- [ ] Host can start shared session and get a link
- [ ] Joiner can connect via browser and see output in real time
- [ ] Host can toggle view-only vs collaborative mode
- [ ] Session requires password/PIN to join
- [ ] Host can kick joiners
- [ ] Transport is encrypted end-to-end
- [ ] Session ends cleanly when host disconnects

---

## 4. Built-in Log Viewer

### Description
Point it at a log file, it tails with syntax highlighting, regex filtering, and bookmarks. Mark interesting lines, jump between errors, collapse stack traces. Better than `tail -f | grep`.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| LV-01 | Tail log files in real time (like `tail -f`) | Must |
| LV-02 | Syntax highlighting for common log formats (JSON, plain text, syslog) | Must |
| LV-03 | Regex filter — show only lines matching pattern | Must |
| LV-04 | Inverse filter — hide lines matching pattern | Should |
| LV-05 | Level-based filtering (ERROR, WARN, INFO, DEBUG) | Must |
| LV-06 | Color-coded log levels (red = ERROR, yellow = WARN, etc.) | Must |
| LV-07 | Bookmark interesting lines | Should |
| LV-08 | Jump between bookmarks | Should |
| LV-09 | Jump between errors (next error / previous error) | Must |
| LV-10 | Collapse/expand stack traces | Should |
| LV-11 | Line count and filtered count display | Must |
| LV-12 | Auto-detect log format from file extension/content | Should |
| LV-13 | Multiple log files in split panes | Should |
| LV-14 | Search within log (forward/backward) | Must |
| LV-15 | Pause tailing (freeze view) | Must |
| LV-16 | Export filtered lines to file | Could |
| LV-17 | Timestamp parsing and display | Should |
| LV-18 | Works with large files (> 1GB) without memory issues | Must |

### Acceptance Criteria

- [ ] `synthshell-log app.log` tails the file in real time
- [ ] Log levels are color-coded
- [ ] Regex filter works
- [ ] Can jump between errors
- [ ] Works with files > 1GB without crashing
- [ ] Pause/resume tailing works

---

## 5. Terminal-Native API Tester

### Description
A mini-Postman inside the terminal. Define requests, save them per-project, see formatted responses with syntax highlighting. Saved requests become command snippets.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| AT-01 | Define HTTP requests (method, URL, headers, body) | Must |
| AT-02 | Support GET, POST, PUT, PATCH, DELETE methods | Must |
| AT-03 | Request editor with syntax highlighting for JSON body | Must |
| AT-04 | Response display with syntax highlighting (JSON, XML, HTML) | Must |
| AT-05 | Response metadata display (status code, time, size) | Must |
| AT-06 | Save requests per-project in `.synthshell/requests/` | Must |
| AT-07 | Save requests globally in `~/.synthshell/requests/` | Must |
| AT-08 | Request groups/folders (e.g., "Auth", "Users", "Products") | Must |
| AT-09 | Environment variable interpolation (`{{GROQ_API_KEY}}`) | Must |
| AT-10 | Request history (last N requests) | Should |
| AT-11 | Import from Postman collection (JSON) | Could |
| AT-12 | Import from cURL command | Must |
| AT-13 | Export as cURL command | Should |
| AT-14 | Cookie jar (persist cookies across requests) | Could |
| AT-15 | Request chaining (use response from one request in next) | Could |
| AT-16 | Saved requests appear in command manager | Should |
| AT-17 | Response body search/filter | Should |

### Acceptance Criteria

- [ ] Can define and execute HTTP requests from terminal
- [ ] Response displayed with syntax highlighting
- [ ] Saved requests work per-project and globally
- [ ] Environment variable interpolation works (`{{API_KEY}}`)
- [ ] Import from cURL works
- [ ] Requests appear in command manager

---

## 6. Voice Commands

### Description
"Hey SynthShell, show me the Docker containers." Or just "run the build." Uses Groq's Whisper integration for speech-to-text. Needs always-listening mode with a hotkey trigger.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| VC-01 | Push-to-talk hotkey (hold to speak, release to process) | Must |
| VC-02 | Always-listening mode with wake word ("Hey SynthShell") | Should |
| VC-03 | Speech-to-text via Groq Whisper API | Must |
| VC-04 | Command matching — map spoken text to saved commands | Must |
| VC-05 | Fuzzy matching for approximate command names | Must |
| VC-06 | Confirmation before execution (show matched command, ask yes/no) | Must |
| VC-07 | Natural language → command translation via Groq LLM | Should |
| VC-08 | Visual indicator when listening (mic icon, pulsing animation) | Must |
| VC-09 | Audio feedback — beep on start listening, beep on recognized | Should |
| VC-10 | Configurable wake word | Could |
| VC-11 | Noise cancellation / sensitivity settings | Should |
| VC-12 | Voice command history (what was said, what was executed) | Should |
| VC-13 | Works with any microphone (select input device) | Must |
| VC-14 | Disable voice entirely (privacy mode) | Must |
| VC-15 | Offline fallback (local Whisper model, lower accuracy) | Could |

### Acceptance Criteria

- [ ] Push-to-talk works with configurable hotkey
- [ ] Speech-to-text accurately transcribes commands
- [ ] Matched commands execute after confirmation
- [ ] Visual indicator shows when listening
- [ ] Can be fully disabled in settings
- [ ] Wake word mode works ("Hey SynthShell")

---

## 7. Terminal Games While Waiting

### Description
Builds take 5 minutes? Play Snake, Tetris, or Minesweeper in a split pane. Sounds dumb but it's the kind of thing that goes viral on Twitter and gets you 10k stars.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| TG-01 | Built-in Snake game | Must |
| TG-02 | Built-in Tetris game | Must |
| TG-03 | Built-in Minesweeper game | Should |
| TG-04 | Launch with command (`synthshell-games`) or hotkey | Must |
| TG-05 | Runs in split pane without blocking main terminal | Must |
| TG-06 | High score persistence per game | Should |
| TG-07 | Theme-aware (game colors match current SynthShell theme) | Must |
| TG-08 | Keyboard controls (arrow keys, space, etc.) | Must |
| TG-09 | Pause/resume game | Must |
| TG-10 | Auto-launch suggestion when command takes > N seconds | Could |
| TG-11 | Leaderboard (local, per-machine) | Could |
| TG-12 | ASCII art rendering for game graphics | Must |
| TG-13 | Sound effects for games (eat, rotate, explode) | Could |
| TG-14 | Additional games via plugin system | Could |

### Acceptance Criteria

- [ ] Snake, Tetris, and Minesweeper playable in terminal
- [ ] Games run in split pane without blocking terminal
- [ ] Games match current theme colors
- [ ] High scores persist across sessions
- [ ] Keyboard controls work correctly
- [ ] Games can be launched and exited cleanly

---

## 8. Plugin Marketplace

### Description
Not just a theme library — a plugin marketplace. Community-built plugins for everything: Kubernetes management, AWS CLI shortcuts, Jira integration, Spotify controls. Turns SynthShell from a terminal into a platform.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| PM-01 | Plugin API (TypeScript/JavaScript) | Must |
| PM-02 | Plugin manifest (name, version, author, description, permissions) | Must |
| PM-03 | Plugin lifecycle hooks (activate, deactivate, onCommand, onThemeChange) | Must |
| PM-04 | Plugin sandboxing (restricted filesystem/network access) | Must |
| PM-05 | Local plugin installation from folder or tarball | Must |
| PM-06 | Remote plugin registry (centralized or self-hosted) | Should |
| PM-07 | `synthshell install <plugin>` CLI command | Must |
| PM-08 | `synthshell list` — show installed plugins | Must |
| PM-09 | `synthshell update <plugin>` — update to latest version | Should |
| PM-10 | Plugin settings UI (per-plugin config page) | Should |
| PM-11 | Plugin permissions model (read, write, network, shell access) | Must |
| PM-12 | Plugin versioning and compatibility checks | Must |
| PM-13 | Community submission workflow (PR-based) | Could |
| PM-14 | Plugin ratings/reviews in marketplace | Could |
| PM-15 | Plugin dependencies (one plugin can depend on another) | Could |
| PM-16 | Hot-reload plugins without terminal restart | Could |
| PM-17 | Plugin development scaffold (`synthshell plugin init`) | Should |
| PM-18 | Plugin documentation template | Should |
| PM-19 | Revenue model for premium plugins (optional, future) | Could |
| PM-20 | Featured plugins section in marketplace UI | Could |

### Acceptance Criteria

- [ ] Plugin API documented and functional
- [ ] Plugins can be installed, listed, and removed
- [ ] Plugin sandboxing prevents unauthorized access
- [ ] Plugin permissions model enforced
- [ ] Plugin scaffold command generates boilerplate
- [ ] Remote registry browseable from terminal

---

## 9. Inline Image Rendering

### Description
Render images directly in the terminal using the Kitty graphics protocol or Sixel format. `cat image.png` shows the image. Preview assets, view Python plots, check generated images — all without leaving the terminal.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| IR-01 | Support Kitty graphics protocol for inline image display | Must |
| IR-02 | Support Sixel format as fallback for non-Kitty terminals | Should |
| IR-03 | Render images from file path (`cat image.png`, `open image.jpg`) | Must |
| IR-04 | Render images from stdin (pipe support) | Should |
| IR-05 | Auto-detect terminal capability (Kitty vs Sixel vs none) | Must |
| IR-06 | Configurable max image size (width/height/bytes) to prevent memory spikes | Must |
| IR-07 | Support PNG, JPEG, GIF (static frame), WebP formats | Must |
| IR-08 | Image rendering does not block terminal input | Must |
| IR-09 | Clear image on screen clear or scroll past viewport | Should |
| IR-10 | Works in split panes (image scales to pane width) | Should |

### Acceptance Criteria

- [ ] `cat image.png` displays the image inline in the terminal
- [ ] Images render without blocking terminal input
- [ ] Large images are scaled down, not rendered at full resolution
- [ ] GIF shows first frame (static)
- [ ] Images clear properly on `clear` command
- [ ] Works in both Kitty and Sixel-capable terminals

---

## 10. Smart History Search

### Description
Enhanced command history search with fuzzy matching, previews, timestamps, and directory context. Ctrl+R but better — find any command you've ever run, even if you only remember fragments.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| SH-01 | Fuzzy search across entire command history (Ctrl+R replacement) | Must |
| SH-02 | Show timestamp for each command in search results | Must |
| SH-03 | Show working directory where command was executed | Must |
| SH-04 | Show shell type (PowerShell, Bash, etc.) for each entry | Should |
| SH-05 | Preview pane — show command output when hovering/selecting entry | Should |
| SH-06 | Filter by date range (today, this week, this month) | Should |
| SH-07 | Filter by directory (commands run in this project) | Should |
| SH-08 | Filter by shell type | Could |
| SH-09 | Search results sorted by relevance + recency | Must |
| SH-10 | Selected command inserted into current input (not executed immediately) | Must |
| SH-11 | Cross-session history (search across all terminal sessions) | Should |
| SH-12 | History stored in SQLite for fast queries on large datasets | Should |
| SH-13 | Sync history across machines via Tailscale or cloud (optional) | Could |

### Acceptance Criteria

- [ ] Ctrl+R opens fuzzy search with full history
- [ ] Search results show timestamp and directory
- [ ] Typing partial commands filters results in real time
- [ ] Selecting a command inserts it without executing
- [ ] History persists across terminal restarts
- [ ] Search is fast even with 100k+ history entries (< 100ms)

---

## 11. Process Monitor

### Description
A sidebar panel showing CPU, RAM, and running processes. Not a full system monitor — just enough to know if your build is eating all your RAM or if a zombie process is lurking.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| PM-01 | Display CPU usage percentage (overall) | Must |
| PM-02 | Display RAM usage (used / total, percentage) | Must |
| PM-03 | Display disk usage for primary drive | Should |
| PM-04 | Top 10 processes by CPU or RAM | Must |
| PM-05 | Process name, PID, CPU%, RAM% per entry | Must |
| PM-06 | Sort by CPU or RAM (toggle) | Should |
| PM-07 | Kill process by PID (with confirmation) | Should |
| PM-08 | Auto-refresh (configurable interval, default 2s) | Must |
| PM-09 | Pause/resume refresh | Should |
| PM-10 | Collapsible panel with hotkey toggle | Must |
| PM-11 | Color-coded usage (green < 50%, yellow 50-80%, red > 80%) | Must |
| PM-12 | Minimal CPU impact for the monitor itself (< 1%) | Must |
| PM-13 | Network usage display (bytes in/out) | Could |

### Acceptance Criteria

- [ ] Panel shows CPU and RAM usage in real time
- [ ] Top processes listed with name, PID, CPU%, RAM%
- [ ] Color-coded thresholds (green/yellow/red)
- [ ] Can kill a process with confirmation dialog
- [ ] Monitor itself uses < 1% CPU
- [ ] Auto-refreshes every 2 seconds

---

*Last updated: 2026-05-22*
*Total features: 11 (1 moved to v1.0, 3 moved from v1.0 to post-v1.0)*
