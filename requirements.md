# SynthShell — Requirements Document

> **Working Name:** SynthShell
> **Platform:** Windows (WSL-compatible)
> **Type:** Cyberpunk terminal emulator with AI integration and remote control
> **Base:** Fork of Tabby (Eugeny/tabby)
> **Status:** Pre-development
> **Last Updated:** 2026-05-20

---

## Table of Contents

1. [Vision](#vision)
2. [Core Features](#core-features)
3. [Feature 1 — Multi-Shell Support](#feature-1--multi-shell-support)
4. [Feature 2 — Theme Library System](#feature-2--theme-library-system)
5. [Feature 3 — Command Management](#feature-3--command-management)
6. [Feature 4 — AI Integration](#feature-4--ai-integration)
7. [Feature 5 — Remote Control](#feature-5--remote-control)
8. [Feature 6 — Cache Guardian](#feature-6--cache-guardian)
9. [Feature 7 — Inline Image Rendering](#feature-7--inline-image-rendering) *(DEFERRED)*
10. [Feature 8 — Git Integration Panel](#feature-8--git-integration-panel)
11. [Feature 9 — Smart History Search](#feature-9--smart-history-search) *(DEFERRED)*
12. [Feature 10 — Session Persistence](#feature-10--session-persistence)
13. [Feature 11 — Inline Markdown Rendering](#feature-11--inline-markdown-rendering)
14. [Feature 12 — SSH Manager](#feature-12--ssh-manager)
15. [Feature 13 — Environment Variable Manager](#feature-13--environment-variable-manager)
16. [Feature 14 — Process Monitor](#feature-14--process-monitor) *(DEFERRED)*
17. [Feature 15 — Custom Sound Effects](#feature-15--custom-sound-effects)
18. [Feature 16 — Natural Language Command Bar](#feature-16--natural-language-command-bar)
19. [Feature 17 — Mood-Based Themes](#feature-17--mood-based-themes)
20. [Feature 18 — ASCII Art Dashboard](#feature-18--ascii-art-dashboard)
21. [Non-Functional Requirements](#non-functional-requirements)
22. [Development Rules](#development-rules)
23. [Work Breakdown Structure](#work-breakdown-structure)
24. [Stumbling Block Log](#stumbling-block-log)
25. [Milestones & GitHub Sync Points](#milestones--github-sync-points)

---

## Vision

SynthShell is a Windows terminal emulator that combines multi-shell flexibility, a cyberpunk aesthetic, per-project command management, lightweight AI integration, and remote phone control — all built for low RAM consumption and maximum security.

## Quick Start

```
# 1. Fork and clone
git clone https://github.com/YOUR_USERNAME/synthshell.git
cd synthshell

# 2. Install dependencies
npm install

# 3. Build
npm run build

# 4. Run
npm start

# 5. Verify
# Terminal should launch on Windows with default shell detection
```

**Base:** Fork of [Eugeny/tabby](https://github.com/Eugeny/tabby) (40k+ stars)
**Runtime:** Electron + TypeScript + node-pty + xterm.js
**Package Manager:** npm (Node), uv (Python — for AI integration only)
**Platform:** Windows only for v1

It is **not** a terminal built from scratch. It stands on the shoulders of Tabby (40k+ stars, Electron, TypeScript) and extends it with four pillars:

1. **Multi-shell switching** — PowerShell, Miniconda/Bash, CMD, Git Bash
2. **Theme Library** — Extensible theme system with cyberpunk as the flagship
3. **Smart Command Management** — Per-project and global saved commands
4. **Groq AI** — Lightweight AI for quick terminal-level questions
5. **Remote Control** — Phone WebUI via WiFi and Tailscale

---

## Core Features

### What SynthShell Inherits from Tabby

- Full terminal emulation (xterm.js, node-pty)
- Multi-shell support (PowerShell, CMD, WSL, Git Bash, Cygwin, MSYS2)
- SSH and serial client
- Plugin architecture (TypeScript/Node.js)
- Tabbed interface with split panes
- Configurable keybindings
- Portable mode (no install required)

### What SynthShell Adds

| Feature | Description |
|---|---|
| Theme Library | Extensible folder-based theme system with palette, skin, and effects layers |
| Cyberpunk Neon | Flagship theme with CRT scanlines, glow, and particle animations |
| Command Manager | Save, group, and recall commands per-project or globally |
| Groq AI Sidebar | Inline AI assistant for quick commands, explanations, and suggestions |
| Remote Control | Phone WebUI via local WiFi (mDNS) and Tailscale (anywhere) |
| Stumbling Block Log | Built-in documentation of every issue encountered and resolved |

---

## Feature 1 — Multi-Shell Support

### Description
Users can open, switch between, and run multiple shells simultaneously. Shells include PowerShell (5 and 7), CMD, Miniconda Prompt, Git Bash, and WSL distributions.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| MS-01 | Shell dropdown to select active shell per tab | Must |
| MS-02 | Support PowerShell 5, PowerShell 7, CMD, Git Bash | Must |
| MS-03 | Support Miniconda/Conda shell with proper activation | Must |
| MS-04 | Support WSL distributions (Ubuntu, etc.) | Should |
| MS-05 | Split panes — multiple shells visible side by side | Must |
| MS-06 | Shell-specific environment variables preserved per tab | Must |
| MS-07 | Hotkey to cycle between open shells | Should |
| MS-08 | Shell profile auto-detection (detect installed shells on first run) | Should |
| MS-09 | Per-shell working directory memory | Could |
| MS-10 | Conda environment selector in shell dropdown | Could |

### Acceptance Criteria

- [ ] User can open PowerShell, Bash, and CMD in separate tabs
- [ ] User can split the terminal into panes running different shells
- [ ] Conda environments activate correctly in both PowerShell and Bash
- [ ] Shell switching preserves the working directory
- [ ] Auto-detection finds all installed shells on first launch

---

## Feature 2 — Theme Library System

### Description
An extensible, folder-based theme system. Each theme is a self-contained directory with a palette (JSON), skin (CSS), optional effects (CSS + JS), and a preview image. The system is designed so new themes can be added by dropping a folder — zero code changes to the terminal core.

### Architecture

```
themes/
├── cyberpunk-neon/         ← Flagship theme
│   ├── theme.json
│   ├── skin.css
│   ├── effects.css
│   ├── effects.js
│   └── preview.png
├── big-hero-6/             ← Future theme
│   ├── theme.json
│   ├── skin.css
│   └── preview.png
├── iron-man/               ← Future theme
│   ├── theme.json
│   ├── skin.css
│   ├── effects.js
│   └── preview.png
└── _default/               ← Fallback theme
    ├── theme.json
    └── skin.css
```

### Theme Schema (theme.json)

```json
{
  "id": "cyberpunk-neon",
  "name": "Cyberpunk Neon",
  "version": "1.0.0",
  "author": "Arman",
  "description": "Neon-soaked dystopian terminal vibes",
  "palette": {
    "background": "#0a0a12",
    "foreground": "#00ff9f",
    "cursor": "#ff2a6d",
    "selection": "#1a1a2e",
    "accent": "#d300c5",
    "black": "#0a0a12",
    "red": "#ff2a6d",
    "green": "#00ff9f",
    "yellow": "#f9f06b",
    "blue": "#00b8ff",
    "magenta": "#d300c5",
    "cyan": "#00ffdf",
    "white": "#e0e0e0",
    "brightBlack": "#4a4a5a",
    "brightRed": "#ff6b9d",
    "brightGreen": "#5affb8",
    "brightYellow": "#fff59d",
    "brightBlue": "#64c8ff",
    "brightMagenta": "#e050d8",
    "brightCyan": "#5affef",
    "brightWhite": "#ffffff"
  },
  "effects": {
    "scanlines": true,
    "glow": true,
    "glowIntensity": 0.8,
    "particles": true,
    "particleColor": "#00ff9f",
    "flicker": false
  },
  "font": {
    "family": "Fira Code",
    "size": 14,
    "weight": "normal"
  }
}
```

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| TL-01 | Theme manager scans `themes/` directory on startup | Must |
| TL-02 | Each theme is a self-contained folder | Must |
| TL-03 | Theme palette maps to xterm.js ANSI colors | Must |
| TL-04 | Skin CSS is injected/removed when theme is selected | Must |
| TL-05 | Effects JS loads only if present (optional animations) | Must |
| TL-06 | Theme preview shown in settings/selector | Should |
| TL-07 | Hot-reload: edit CSS, see changes without restart | Could |
| TL-08 | Per-shell theme override (different theme per tab) | Could |
| TL-09 | Theme import/export (share themes as zip) | Could |
| TL-10 | Community theme submission workflow | Could |

### Acceptance Criteria

- [ ] Dropping a new theme folder in `themes/` makes it available in the selector
- [ ] Cyberpunk Neon theme renders with CRT scanlines and glow effects
- [ ] Switching themes is instant (no restart)
- [ ] `theme.json` schema is documented and validated
- [ ] Preview image displays in the theme selector
- [ ] Effects layer has zero CPU impact when effects are disabled

---

## Feature 3 — Command Management

### Description
Users can save, organize, and recall terminal commands. Commands are scoped globally (available everywhere) or per-project (auto-detected by working directory). Commands are accessible via a palette, hotkey, or the phone WebUI.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| CM-01 | Command palette (Ctrl+Shift+P) for saved commands | Must |
| CM-02 | Global command storage (`~/.synthshell/commands/global.json`) | Must |
| CM-03 | Per-project command storage (`.synthshell/commands.json` in project root) | Must |
| CM-04 | Auto-detect project from cwd (looks for `.synthshell/`, `.git/`, `package.json`, etc.) | Must |
| CM-05 | Command groups/folders (e.g., "Build", "Deploy", "Test") | Must |
| CM-06 | Commands have name, description, and the actual command string | Must |
| CM-07 | Quick-access sidebar with saved commands | Should |
| CM-08 | Command can include placeholders (`{file}`, `{branch}`, `{env}`) | Should |
| CM-09 | Command history search with fuzzy matching | Should |
| CM-10 | Import/export command sets (share with team) | Could |
| CM-11 | Command execution from phone WebUI | Must |
| CM-12 | Commands appear in phone WebUI grouped by project | Must |

### Command Schema

```json
{
  "commands": [
    {
      "id": "build-prod",
      "name": "Build Production",
      "description": "Run production build with uv",
      "command": "uv run python -m build --prod",
      "group": "Build",
      "shell": "powershell",
      "workingDirectory": null,
      "tags": ["build", "production"]
    }
  ]
}
```

### Acceptance Criteria

- [ ] User can add a command via the UI or by editing the JSON
- [ ] Commands auto-scope to the current project directory
- [ ] Global commands are available regardless of project
- [ ] Command palette supports fuzzy search
- [ ] Placeholders prompt the user for values before execution
- [ ] Commands are accessible from the phone WebUI

---

## Feature 4 — AI Integration

### Description
A lightweight AI sidebar powered by Groq API for fast inference. Designed for quick terminal-level questions — "what does this error mean?", "write a git command to squash last 3 commits", "explain this regex". Not a full coding assistant — a quick reference.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| AI-01 | Groq API integration via API key | Must |
| AI-02 | Sidebar panel (toggle with hotkey) | Must |
| AI-03 | Chat interface — type question, get answer | Must |
| AI-04 | Context-aware — can reference current terminal output | Should |
| AI-05 | Command suggestion — AI suggests a command, user can insert directly | Should |
| AI-06 | API key stored securely (encrypted, not in plaintext config) | Must |
| AI-07 | Model selection (llama, mixtral, gemma via Groq) | Should |
| AI-08 | Token/cost tracking per session | Could |
| AI-09 | Conversation history persists per session | Should |
| AI-10 | Works from phone WebUI (same AI, remote access) | Could |
| AI-11 | Fail-fast API key validation — check key exists at init, not at first query | Must |
| AI-12 | Brace-matching JSON extraction from AI responses (not simple regex) | Must |
| AI-13 | Use API's native JSON mode (`response_format={"type": "json_object"}`) when available | Should |
| AI-14 | Embed command schema in prompt for structured extraction (not natural language description) | Should |
| AI-15 | Add negative constraints to prompts ("Never invent commands. Only suggest what is valid.") | Must |

### Best Practices (from project lessons learned)

> **JSON Extraction:** Groq models may return chain-of-thought reasoning before JSON. Use brace-matching to extract JSON objects — scan for `{`, track depth, collect complete objects, return the longest. Never rely on simple `removeprefix("```json")`.
>
> **API Key Management:** Store Groq key in `.env` or secure configuration, validate at init by throwing a clear initialization `Error` if missing. Never check for the key inside each method — fail fast at startup.
>
> **Structured Output:** When asking Groq to suggest commands, embed the JSON schema directly in the prompt (`JSON.stringify(commandSchema)`). Models follow schemas more reliably than prose descriptions.

### Acceptance Criteria

- [ ] User enters Groq API key in settings
- [ ] AI sidebar opens with hotkey, accepts questions
- [ ] AI can suggest commands that are inserted into the terminal with one click
- [ ] API key is never stored in plaintext
- [ ] API key is validated at init — missing key shows clear error, not a crash deep in the call stack
- [ ] JSON extraction works when AI returns mixed conversation + JSON (brace-matching, not regex)
- [ ] Response time feels instant (< 1s for typical queries via Groq)
- [ ] AI works identically from phone WebUI

---

## Feature 5 — Remote Control

### Description
Phone-based WebUI that can send commands to the terminal and view output in real time. Two connection modes:

1. **Same WiFi** — Auto-discovery via mDNS, instant connection
2. **Anywhere** — Via Tailscale mesh VPN, works from any network

### Architecture

```
┌─────────────────────────────────────────┐
│           SynthShell Terminal (PC)           │
│                                         │
│  ┌──────────┐  ┌──────────────────────┐ │
│  │ Terminal  │  │  Remote Command API  │ │
│  │  Core     │←→│  WebSocket :7777     │ │
│  │ (xterm)   │  │  mDNS broadcast      │ │
│  └──────────┘  │  Tailscale listener  │ │
│                └──────────┬───────────┘ │
└───────────────────────────┼─────────────┘
                            │
            ┌───────────────┼───────────────┐
            │               │               │
     Same WiFi         Tailscale        Tailscale
     (local IP)        (100.x.x.x)      (relay)
            │               │               │
            └───────┬───────┘               │
                    │                       │
              ┌─────┴─────┐                 │
              │  Phone     │←───────────────┘
              │  Web UI    │
              │  (browser) │
              └───────────┘
```

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| RC-01 | WebSocket server on configurable port (default 7777) | Must |
| RC-02 | mDNS auto-discovery for same-WiFi connections | Must |
| RC-03 | Tailscale integration for internet-wide access | Must |
| RC-04 | Auto-detect: try local IP first, fallback to Tailscale | Must |
| RC-05 | Mobile-optimized WebUI (responsive, touch-friendly) | Must |
| RC-06 | Send commands from phone to terminal | Must |
| RC-07 | View terminal output in real time on phone | Must |
| RC-08 | Shell selector on phone (switch between shells) | Should |
| RC-09 | Saved commands accessible from phone WebUI | Must |
| RC-10 | QR code for easy pairing (displays on PC, scan on phone) | Should |
| RC-11 | API key or Tailscale identity authentication | Must |
| RC-12 | Connection status indicator on both PC and phone | Should |
| RC-13 | Command history visible on phone | Could |
| RC-14 | Dark cyberpunk theme on phone WebUI matching terminal | Should |

### Security Requirements

| ID | Requirement | Priority |
|---|---|---|
| RC-S01 | All WebSocket connections authenticated (API key or token) | Must |
| RC-S02 | No commands execute without explicit user approval mode | Should |
| RC-S03 | Rate limiting on command execution (prevent abuse) | Must |
| RC-S04 | Connection logging (who connected, when, what commands) | Must |
| RC-S05 | Kill switch — instantly disable remote access from PC | Must |
| RC-S06 | Tailscale identity verification (only known devices) | Must |
| RC-S07 | Encrypted transport (WSS for local, Tailscale handles internet) | Must |

### Acceptance Criteria

- [ ] Phone auto-discovers terminal on same WiFi via QR code
- [ ] Phone connects to terminal from anywhere via Tailscale
- [ ] Commands sent from phone execute in the terminal
- [ ] Terminal output streams to phone in real time
- [ ] Authentication required before any command execution
- [ ] Kill switch disables remote access instantly
- [ ] Connection log shows all remote sessions

---

## Feature 6 — Cache Guardian

> Inspired by [CrunchyCleaner](https://github.com/knuspii/crunchycleaner) (Knuspii, GPL-3.0). Cache path registry adapted from its `getPrograms()` function.

### Description
A built-in cache monitoring and cleanup system. SynthShell tracks cache sizes for dev tools (uv, pip, npm, cargo, go, etc.), browsers, and system temp folders. Users set per-cache thresholds with color-coded warnings (green/yellow/red) and optional auto-cleanup when limits are exceeded.

### Architecture

```
┌─────────────────────────────────────────────┐
│              Cache Guardian Panel            │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  Cache Registry (JSON)              │    │
│  │  - name, paths, type, commands      │    │
│  │  - threshold config per cache       │    │
│  └──────────┬──────────────────────────┘    │
│             │                               │
│  ┌──────────▼──────────────────────────┐    │
│  │  Scanner (periodic / on-demand)     │    │
│  │  - Calculate directory sizes        │    │
│  │  - Compare against thresholds       │    │
│  │  - Emit status: green / yellow/red  │    │
│  └──────────┬──────────────────────────┘    │
│             │                               │
│  ┌──────────▼──────────────────────────┐    │
│  │  Cleaner (manual / auto)            │    │
│  │  - Run native cleanup commands      │    │
│  │  - Delete directories directly      │    │
│  │  - Log freed space                  │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │  UI: Slider + Status Bar + Panel    │    │
│  │  - Per-cache size bars              │    │
│  │  - Threshold sliders (drag to set)  │    │
│  │  - Red flash notification on breach │    │
│  │  - One-click clean button           │    │
│  └─────────────────────────────────────┘    │
└─────────────────────────────────────────────┘
```

### Cache Registry

Each cache target is defined as a JSON entry. The registry ships with defaults but is fully user-extensible.

```json
{
  "caches": [
    {
      "id": "uv-cache",
      "name": "uv Cache",
      "category": "Package Managers",
      "paths": ["~/.cache/uv", "%LOCALAPPDATA%/uv/cache"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "uv cache clean",
      "description": "Python package cache managed by uv",
      "icon": "🐍"
    },
    {
      "id": "pip-cache",
      "name": "pip Cache",
      "category": "Package Managers",
      "paths": ["~/.cache/pip", "%LOCALAPPDATA%/pip/Cache"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "pip cache purge",
      "description": "Python package cache managed by pip",
      "icon": "🐍"
    },
    {
      "id": "npm-cache",
      "name": "npm Cache",
      "category": "Package Managers",
      "paths": ["~/.npm/_cacache", "%APPDATA%/npm-cache/_cacache"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "npm cache clean --force",
      "description": "Node.js package cache managed by npm",
      "icon": "📦"
    },
    {
      "id": "cargo-cache",
      "name": "Cargo Cache",
      "category": "Package Managers",
      "paths": ["~/.cargo/registry/cache", "~/.cargo/git/db"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "cargo cache -a",
      "description": "Rust package cache managed by Cargo",
      "icon": "🦀"
    },
    {
      "id": "go-build-cache",
      "name": "Go Build Cache",
      "category": "Package Managers",
      "paths": ["~/.cache/go-build", "%LOCALAPPDATA%/go-build"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "go clean -cache",
      "description": "Go build cache",
      "icon": "🐹"
    },
    {
      "id": "yarn-cache",
      "name": "Yarn Cache",
      "category": "Package Managers",
      "paths": ["~/.cache/yarn", "%LOCALAPPDATA%/Yarn/Cache", "%APPDATA%/Yarn/Cache"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "yarn cache clean",
      "description": "Node.js package cache managed by Yarn",
      "icon": "📦"
    },
    {
      "id": "gradle-cache",
      "name": "Gradle Cache",
      "category": "Package Managers",
      "paths": ["~/.gradle/caches"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "gradle --stop && rm -rf ~/.gradle/caches",
      "description": "Java/Kotlin build cache managed by Gradle",
      "icon": "☕"
    },
    {
      "id": "nuget-cache",
      "name": "NuGet Cache",
      "category": "Package Managers",
      "paths": ["~/.nuget/packages"],
      "platforms": ["windows", "linux"],
      "cleanCommand": "dotnet nuget locals all --clear",
      "description": ".NET package cache managed by NuGet",
      "icon": "🟣"
    },
    {
      "id": "vscode-cache",
      "name": "VS Code Cache",
      "category": "IDEs",
      "paths": [
        "%APPDATA%/Code/Cache",
        "%APPDATA%/Code/CachedData",
        "%APPDATA%/Code/CachedExtensionVSIXs",
        "%APPDATA%/Code/GPUCache",
        "%APPDATA%/Code/User/workspaceStorage"
      ],
      "platforms": ["windows"],
      "cleanCommand": null,
      "description": "VS Code editor cache and workspace storage",
      "icon": "💻"
    },
    {
      "id": "jetbrains-cache",
      "name": "JetBrains IDE Cache",
      "category": "IDEs",
      "paths": ["%LOCALAPPDATA%/JetBrains/*/system/caches"],
      "platforms": ["windows"],
      "cleanCommand": null,
      "description": "JetBrains IDE cache (IntelliJ, PyCharm, WebStorm)",
      "icon": "🧠"
    },
    {
      "id": "chrome-cache",
      "name": "Chrome Cache",
      "category": "Browsers",
      "paths": [
        "%LOCALAPPDATA%/Google/Chrome/User Data/Default/Cache",
        "%LOCALAPPDATA%/Google/Chrome/User Data/Default/Code Cache",
        "~/.cache/google-chrome"
      ],
      "platforms": ["windows", "linux"],
      "cleanCommand": null,
      "description": "Google Chrome browser cache",
      "icon": "🌐"
    },
    {
      "id": "firefox-cache",
      "name": "Firefox Cache",
      "category": "Browsers",
      "paths": [
        "%LOCALAPPDATA%/Mozilla/Firefox/Profiles/*/cache2",
        "~/.cache/mozilla/firefox/*/cache2"
      ],
      "platforms": ["windows", "linux"],
      "cleanCommand": null,
      "description": "Mozilla Firefox browser cache",
      "icon": "🦊"
    },
    {
      "id": "discord-cache",
      "name": "Discord Cache",
      "category": "Apps",
      "paths": [
        "%APPDATA%/discord/Cache",
        "%APPDATA%/discord/Code Cache",
        "%APPDATA%/discord/GPUCache"
      ],
      "platforms": ["windows"],
      "cleanCommand": null,
      "description": "Discord application cache",
      "icon": "💬"
    },
    {
      "id": "temp-folders",
      "name": "Temp Folders",
      "category": "System",
      "paths": ["%LOCALAPPDATA%/Temp", "/tmp"],
      "platforms": ["windows", "linux"],
      "cleanCommand": null,
      "description": "System temporary files",
      "icon": "🗑️"
    },
    {
      "id": "nvidia-gl-cache",
      "name": "NVIDIA Shader Cache",
      "category": "System",
      "paths": ["%LOCALAPPDATA%/NVIDIA/GLCache", "%LOCALAPPDATA%/D3DSCache"],
      "platforms": ["windows"],
      "cleanCommand": null,
      "description": "NVIDIA GPU shader cache",
      "icon": "🎮"
    }
  ]
}
```

### Threshold Configuration

Users set thresholds per cache or per category. Three states: green (under limit), yellow (approaching), red (over limit).

```json
{
  "thresholds": {
    "defaults": {
      "yellow_mb": 500,
      "red_mb": 2000,
      "auto_clean": false
    },
    "overrides": {
      "uv-cache": { "yellow_mb": 1000, "red_mb": 5000, "auto_clean": false },
      "npm-cache": { "yellow_mb": 300, "red_mb": 1000, "auto_clean": true },
      "temp-folders": { "yellow_mb": 1000, "red_mb": 3000, "auto_clean": true }
    }
  }
}
```

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| CG-01 | Cache registry with 15+ dev/browser/system caches (Windows) | Must |
| CG-02 | User-extensible registry (add custom cache paths via JSON) | Must |
| CG-03 | Scan cache sizes on demand (manual trigger) | Must |
| CG-04 | Periodic background scan (configurable interval, default 6h) | Should |
| CG-05 | Per-cache threshold sliders (yellow/red limits in MB) | Must |
| CG-06 | Category-level thresholds (all Package Managers share a limit) | Should |
| CG-07 | Color-coded status: green bar → yellow bar → red bar | Must |
| CG-08 | Red notification banner when any cache exceeds its limit | Must |
| CG-09 | Desktop notification (Windows toast) on threshold breach | Should |
| CG-10 | One-click clean button per cache | Must |
| CG-11 | Batch clean — clean all red caches at once | Must |
| CG-12 | Auto-clean option — automatically clean when red threshold hit | Must |
| CG-13 | Use native clean commands where available (uv cache clean, npm cache clean, etc.) | Must |
| CG-14 | Fallback: direct directory deletion when no native command exists | Must |
| CG-15 | Dry-run mode — show what would be deleted without deleting | Should |
| CG-16 | Cleanup log — track what was cleaned, when, how much freed | Must |
| CG-17 | Cache Guardian panel in sidebar (collapsible) | Must |
| CG-18 | Visual slider UI for setting thresholds (drag to adjust MB) | Must |
| CG-19 | Total cache size summary at top of panel | Should |
| CG-20 | Platform-aware paths (resolve ~ and %LOCALAPPDATA% correctly) | Must |
| CG-21 | Lazy load — scanner does not run until panel is opened or interval fires | Should |
| CG-22 | Account for symlinks in size calculation (Windows Developer Mode affects HuggingFace cache) | Could |

### UI Mockup (Cache Guardian Panel)

```
╔══════════════════════════════════════════════════╗
║  🧹 Cache Guardian                    12.4 GB    ║
╠══════════════════════════════════════════════════╣
║                                                  ║
║  Package Managers                                ║
║  ─────────────────────────────────────────────   ║
║  🐍 uv Cache          ████████████░░  4.2 GB     ║
║     [====yellow====|===red===]        [Clean]    ║
║                                                  ║
║  📦 npm Cache         █████░░░░░░░░░  1.1 GB     ║
║     [===yellow===|==red==]            [Clean]    ║
║                                                  ║
║  🐍 pip Cache         ██░░░░░░░░░░░░  340 MB     ║
║     [==yellow==|=red=]                [Clean]    ║
║                                                  ║
║  Browsers                                        ║
║  ─────────────────────────────────────────────   ║
║  🌐 Chrome Cache      ████████░░░░░░  2.8 GB     ║
║     [====yellow====|===red===]        [Clean]    ║
║                                                  ║
║  ─────────────────────────────────────────────   ║
║  ⚠️  uv cache is over limit (4.2 GB > 3 GB)     ║
║                                                  ║
║  [🧹 Clean All Red]  [🔄 Rescan]  [⚙️ Settings] ║
╚══════════════════════════════════════════════════╝
```

### Acceptance Criteria

- [ ] Cache registry lists 15+ caches with correct Windows paths
- [ ] User can add custom cache entries via JSON file
- [ ] Scanner correctly calculates directory sizes (including nested dirs)
- [ ] Threshold sliders update in real time and persist to config
- [ ] Red notification appears when any cache exceeds its limit
- [ ] One-click clean uses native commands (uv cache clean, npm cache clean, etc.)
- [ ] Auto-clean triggers within 1 minute of threshold breach
- [ ] Cleanup log records timestamp, cache name, size before, size after, bytes freed
- [ ] Dry-run mode lists files that would be deleted without deleting
- [ ] Platform-aware: resolves Windows env vars and Linux ~ paths correctly
- [ ] Background scanner does not impact terminal performance (< 2% CPU)

---

## Feature 7 — Inline Image Rendering

> **Status:** Moved to post-v1.0 (see late-improvements.md, Feature 9)

---

## Feature 8 — Git Integration Panel

### Description
A sidebar panel showing essential git information at a glance: current branch, changed files, quick diff preview, and one-click stage/commit. Not a full GUI git client — just the 5 things developers actually need.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| GP-01 | Display current branch name in panel header | Must |
| GP-02 | List changed/staged/untracked files with status icons (M, A, D, ?) | Must |
| GP-03 | Click file to preview diff in split view | Must |
| GP-04 | One-click stage/unstage individual files | Must |
| GP-05 | Commit message input with one-click commit | Must |
| GP-06 | Ahead/behind indicator vs remote (e.g., "↑2 ↓1") | Should |
| GP-07 | Branch switcher dropdown | Should |
| GP-08 | Stash list with pop/apply/drop actions | Could |
| GP-09 | Git status summary at top (e.g., "3 modified, 1 new, 2 untracked") | Must |
| GP-10 | Auto-detect git repos from cwd (show panel only in git directories) | Must |
| GP-11 | Collapsible panel — toggle with hotkey | Must |
| GP-12 | Color-coded file status (green = staged, red = modified, grey = untracked) | Must |

### Acceptance Criteria

- [ ] Panel shows current branch and file status when in a git directory
- [ ] Clicking a file shows its diff
- [ ] One-click stage, unstage, and commit work
- [ ] Panel auto-hides when not in a git directory
- [ ] Ahead/behind remote indicator displays correctly
- [ ] Panel does not slow down terminal in large repos (< 100ms refresh)

---

## Feature 9 — Smart History Search

> **Status:** Moved to post-v1.0 (see late-improvements.md, Feature 10)

---

## Feature 10 — Session Persistence

### Description
Terminal state survives crashes, power loss, and accidental closes. On reopen: every tab, pane, shell type, working directory, and scrollback is restored exactly as it was.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| SP-01 | Auto-save session state on interval (configurable, default 30s) | Must |
| SP-02 | Save state on graceful close (window close, exit command) | Must |
| SP-03 | Restore all tabs on reopen | Must |
| SP-04 | Restore all panes within each tab | Must |
| SP-05 | Restore shell type per tab/pane | Must |
| SP-06 | Restore working directory per tab/pane | Must |
| SP-07 | Restore scrollback buffer (configurable depth) | Should |
| SP-08 | Restore active tab/pane focus | Must |
| SP-09 | Handle crash recovery (detect unclean shutdown, offer restore) | Must |
| SP-10 | Multiple session profiles (e.g., "Work", "Personal", "Project X") | Could |
| SP-11 | Session state stored in `~/.synthshell/sessions/` as JSON | Must |
| SP-12 | Encrypted session state if it contains sensitive env vars | Should |
| SP-13 | Configurable: disable persistence for specific tabs (e.g., temp shells) | Could |

### Acceptance Criteria

- [ ] Closing and reopening terminal restores all tabs and panes
- [ ] Crash recovery detects unclean shutdown and offers restore
- [ ] Each tab/pane has correct shell type and working directory after restore
- [ ] Session state file is < 1MB for typical usage (10 tabs, 5 panes)
- [ ] Restore takes < 2 seconds on startup
- [ ] Sensitive env vars are encrypted in session state

---

## Feature 11 — Inline Markdown Rendering

### Description
Render markdown files in the terminal with styled output: headers, bold/italic, syntax-highlighted code blocks, tables, and lists. Not just `cat` with monochrome — actually readable, styled output.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| MD-01 | Render markdown headers with styling (bold, colored, sized) | Must |
| MD-02 | Render bold, italic, strikethrough with terminal styling | Must |
| MD-03 | Render code blocks with syntax highlighting (language-aware) | Must |
| MD-04 | Render inline code with background highlight | Must |
| MD-05 | Render tables with aligned columns | Must |
| MD-06 | Render ordered and unordered lists with proper indentation | Must |
| MD-07 | Render links as clickable (OSC 8 hyperlinks) or styled text | Should |
| MD-08 | Render blockquotes with left-border styling | Should |
| MD-09 | Support `view` or `md` command to render a markdown file | Must |
| MD-10 | Pipe support: `cat README.md \| synthshell-md` | Should |
| MD-11 | Syntax highlighting uses current theme's color palette | Must |
| MD-12 | Configurable: toggle markdown rendering per file extension | Could |
| MD-13 | Works with large files (paginated output, not render-all) | Should |

### Acceptance Criteria

- [ ] `view README.md` renders styled markdown in the terminal
- [ ] Code blocks have syntax highlighting for common languages (JS, Python, Go, Rust, JSON, YAML)
- [ ] Tables are aligned and readable
- [ ] Rendered output respects the current theme colors
- [ ] Large files paginate correctly
- [ ] `cat README.md` still shows raw markdown (rendering is opt-in)

---

## Feature 12 — SSH Manager

### Description
Saved SSH profiles with icons, groups, and quick-connect. Server status indicators, connection history, and auto-reconnect. A bookmarks bar for servers.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| SM-01 | Save SSH profiles (host, port, user, key path, label) | Must |
| SM-02 | Profile groups (e.g., "Production", "Staging", "Personal") | Must |
| SM-03 | Quick-connect via profile name or hotkey | Must |
| SM-04 | Server status indicator (green = reachable, red = down, grey = unknown) | Should |
| SM-05 | Connection history (last connected, connection count) | Should |
| SM-06 | Auto-reconnect on disconnect (configurable retry) | Should |
| SM-07 | Custom icons per profile or group | Could |
| SM-08 | Import from `~/.ssh/config` | Must |
| SM-09 | Export profiles to JSON for backup/sharing | Could |
| SM-10 | Encrypted storage of SSH credentials (keychain integration) | Must |
| SM-11 | Profile search/filter in sidebar | Should |
| SM-12 | Port forwarding configuration per profile | Could |

### Acceptance Criteria

- [ ] SSH profiles saved and accessible from sidebar
- [ ] Import from `~/.ssh/config` works
- [ ] Quick-connect with hotkey or profile name
- [ ] Server status shows reachability
- [ ] Credentials stored securely (not plaintext)

---

## Feature 13 — Environment Variable Manager

### Description
A centralized panel for managing environment variables and API keys. Variables are stored globally and can be copied to clipboard for use in other projects. Solves the "notepad full of API keys" problem. Variables are scoped: global (always available) or per-project (auto-loaded based on cwd).

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| EV-01 | Centralized env var storage in `~/.synthshell/env/` (encrypted at rest) | Must |
| EV-02 | Global env vars — available in all shells automatically | Must |
| EV-03 | Per-project env vars — auto-loaded when cwd matches project | Must |
| EV-04 | Auto-detect project from `.synthshell/env.json`, `.env`, `.git/`, `pyproject.toml` | Must |
| EV-05 | Add/edit/delete env vars via UI panel | Must |
| EV-06 | Copy any env var value to clipboard with one click | Must |
| EV-07 | Copy full `.env` file content to clipboard | Must |
| EV-08 | Copy individual var as `export KEY=value` (Bash) or `$env:KEY="value"` (PowerShell) format | Must |
| EV-09 | Masked display for sensitive values (API keys shown as `sk-or-...7df8`) | Must |
| EV-10 | Toggle reveal/hide for individual values | Must |
| EV-11 | Search/filter env vars by name or tag | Must |
| EV-12 | Tags for env vars (e.g., "AI", "Cloud", "Database", "Dev") | Should |
| EV-13 | Import from existing `.env` files (drag-drop or path input) | Must |
| EV-14 | Export as `.env` file for a specific project | Must |
| EV-15 | Env var history — track value changes over time | Could |
| EV-16 | Quick-paste: paste an API key and auto-detect the var name (e.g., `GROQ_API_KEY`) | Should |
| EV-17 | Bulk import — paste multiple `KEY=value` lines at once | Should |
| EV-18 | Inject env vars into new shell sessions automatically | Must |
| EV-19 | Env vars accessible from phone WebUI (copy key from phone) | Should |
| EV-20 | Collapsible sidebar panel with category groups | Must |

### Env Storage Schema

```json
{
  "global": {
    "GROQ_API_KEY": { "value": "gsk_...abc", "masked": true, "tags": ["AI"], "updated": "2026-05-20" },
    "TAVILY_API_KEY": { "value": "tvly-...xyz", "masked": true, "tags": ["AI", "Search"], "updated": "2026-05-19" },
    "GITHUB_TOKEN": { "value": "ghp_...123", "masked": true, "tags": ["Dev"], "updated": "2026-05-18" }
  },
  "projects": {
    "synthshell": {
      "match": ["~/projects/synthshell", ".git:origin.*synthshell"],
      "vars": {
        "NODE_ENV": { "value": "development", "masked": false, "tags": ["Dev"] }
      }
    }
  }
}
```

### UI Mockup

```
╔══════════════════════════════════════════════════╗
║  🔑 Environment Variables                        ║
╠══════════════════════════════════════════════════╣
║  [🔍 Search vars...]           [+ Add] [Import] ║
║                                                  ║
║  🌐 Global (5 vars)                              ║
║  ─────────────────────────────────────────────   ║
║  🤖 AI                                           ║
║  GROQ_API_KEY        gsk_...abc  [👁][📋][✏️]   ║
║  TAVILY_API_KEY      tvly-...xyz [👁][📋][✏️]   ║
║  GOOGLE_API_KEY      AIza...LUFQ [👁][📋][✏️]   ║
║                                                  ║
║  🔧 Dev                                          ║
║  GITHUB_TOKEN        ghp_...123  [👁][📋][✏️]   ║
║  OPENAI_API_KEY      sk-...def   [👁][📋][✏️]   ║
║                                                  ║
║  📁 Project: synthshell (3 vars)                      ║
║  ─────────────────────────────────────────────   ║
║  NODE_ENV            development [👁][📋][✏️]   ║
║  DEBUG               true        [👁][📋][✏️]   ║
║                                                  ║
║  [📋 Copy All as .env]  [📋 Copy All as export]  ║
╚══════════════════════════════════════════════════╝
```

### Acceptance Criteria

- [ ] Global env vars automatically available in all new shell sessions
- [ ] Per-project env vars auto-load when cwd matches project
- [ ] One-click copy any value to clipboard
- [ ] Copy as `export KEY=value` for Bash or `$env:KEY="value"` for PowerShell
- [ ] Import from existing `.env` file
- [ ] Export as `.env` file for a project
- [ ] API keys masked by default, reveal on click
- [ ] Values encrypted at rest in `~/.synthshell/env/`
- [ ] Search finds vars by name or tag
- [ ] Bulk import from pasted `KEY=value` block

---

## Feature 14 — Process Monitor

> **Status:** Moved to post-v1.0 (see late-improvements.md, Feature 11)

---

## Feature 15 — Custom Sound Effects

### Description
Terminal events trigger sounds. Command success = subtle chime. Error = low buzz. Build complete = satisfying ping. All configurable, all toggleable. Sounds provide audio feedback without looking at the screen.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| SE-01 | Sound events: command success, command error, build complete, notification | Must |
| SE-02 | Per-event enable/disable toggle | Must |
| SE-03 | Volume control (master + per-event) | Must |
| SE-04 | Custom sound file per event (user can upload .wav/.mp3) | Should |
| SE-05 | Built-in sound pack (3-5 options per event) | Should |
| SE-06 | Mute all hotkey (instant silence) | Must |
| SE-07 | Quiet hours — disable sounds during configurable time range | Should |
| SE-08 | Sound on threshold breach (Cache Guardian red alert) | Should |
| SE-09 | Sound on remote connection (phone connects) | Could |
| SE-10 | No sound by default — opt-in only | Must |
| SE-11 | Sound files stored in `~/.synthshell/sounds/` | Must |
| SE-12 | Works on Windows (native audio, no external dependencies) | Must |

### Sound Event Mapping

| Event | Default Sound | Trigger |
|---|---|---|
| Command success | Soft chime (high) | Exit code 0 |
| Command error | Low buzz | Non-zero exit code |
| Build complete | Ascending tones | `make`, `npm run build`, `uv build` completes |
| Notification | Gentle ping | Desktop notification or AI response |
| Cache alert | Warning tone | Cache Guardian threshold breach |
| Remote connect | Subtle click | Phone WebUI connects |

### Acceptance Criteria

- [ ] Sounds play on command success/error
- [ ] Per-event toggle in settings
- [ ] Volume control works
- [ ] Custom sound files can be uploaded per event
- [ ] Mute all hotkey works instantly
- [ ] No sounds by default (opt-in)
- [ ] Works on Windows without external audio libraries

---

## Feature 16 — Natural Language Command Bar

### Description
A secondary input bar (like Spotlight on macOS) where you type natural language and get translated terminal commands. "find all PDFs modified today larger than 10MB" → `find . -name "*.pdf" -mtime 0 -size +10M`. Uses Groq for fast translation. Not for experts — for the commands you Google every time.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| NL-01 | Open natural language bar with hotkey (Ctrl+Shift+N or similar) | Must |
| NL-02 | Type natural language description of what you want | Must |
| NL-03 | Groq translates to terminal command | Must |
| NL-04 | Show translated command with explanation | Must |
| NL-05 | "Run" button to execute directly | Must |
| nl-06 | "Copy" button to copy command to clipboard | Must |
| NL-07 | "Edit" button to modify before running | Must |
| NL-08 | Include current shell context (PowerShell vs Bash syntax) | Must |
| NL-09 | Include current directory context (project type, files present) | Should |
| NL-10 | Multi-step commands: "find JS files, count lines, sort by size" → pipeline | Should |
| NL-11 | History of natural language queries and their translations | Should |
| NL-12 | Works from phone WebUI | Could |
| NL-13 | Fallback: if Groq unavailable, show "No AI connection" | Must |
| NL-14 | Command validation — warn if translated command looks dangerous (rm -rf, etc.) | Must |

### Acceptance Criteria

- [ ] Hotkey opens natural language bar
- [ ] Typing "find large files" returns a valid `find` or equivalent command
- [ ] Translated command matches current shell (PowerShell vs Bash)
- [ ] Dangerous commands flagged with warning
- [ ] Can run, copy, or edit the translated command
- [ ] Response time < 2s via Groq

---

## Feature 17 — Mood-Based Themes

### Description
Terminal changes theme automatically based on context and time. A configurable option — when enabled, SynthShell shifts themes throughout the day or in response to events. Each theme defines its own mood schedule.

### Behavior

| Trigger | Theme Shift | Example |
|---|---|---|
| Time: Morning (6am-12pm) | Calm, cool tones | Blue/teal palette |
| Time: Afternoon (12pm-6pm) | Neutral, productive | Grey/white palette |
| Time: Evening (6pm-10pm) | Warm, relaxed | Amber/orange palette |
| Time: Night (10pm-6am) | Cyberpunk neon | Full neon glow |
| Event: Build success | Brief green pulse | Green accent flash |
| Event: Build failure | Brief red pulse | Red accent flash |
| Event: Error in terminal | Subtle red tint | Red-shifted palette |
| Event: All clear (no errors) | Return to base mood | Revert to time-based |

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| MT-01 | Mood-based theme toggle (opt-in, off by default) | Must |
| MT-02 | Each theme can define a mood schedule (time-based palette shifts) | Must |
| MT-03 | Default mood schedule: morning calm → afternoon neutral → evening warm → night neon | Must |
| MT-04 | User-customizable time ranges for each mood | Must |
| MT-05 | Event triggers: build success/failure, error, all-clear | Should |
| MT-06 | Smooth transition between moods (CSS transition, not instant swap) | Should |
| MT-07 | Mood indicator in status bar (current mood name + icon) | Should |
| MT-08 | Override: manually lock to a specific mood | Must |
| MT-09 | Each theme's `theme.json` can define `mood_schedule` object | Must |
| MT-10 | Fallback: if no mood schedule defined, use default | Must |
| MT-11 | Mood changes logged (for debugging) | Could |

### Theme Schema Extension

```json
{
  "mood_schedule": {
    "enabled": true,
    "moods": {
      "morning": {
        "time": "06:00-12:00",
        "palette_override": {
          "background": "#0a1628",
          "foreground": "#7ec8e3",
          "accent": "#4fc3f7"
        }
      },
      "afternoon": {
        "time": "12:00-18:00",
        "palette_override": {
          "background": "#1a1a2e",
          "foreground": "#e0e0e0",
          "accent": "#9e9e9e"
        }
      },
      "evening": {
        "time": "18:00-22:00",
        "palette_override": {
          "background": "#1a0a0a",
          "foreground": "#ffb74d",
          "accent": "#ff8a65"
        }
      },
      "night": {
        "time": "22:00-06:00",
        "palette_override": {}
      }
    },
    "events": {
      "build_success": { "accent": "#00ff9f", "duration_seconds": 3 },
      "build_failure": { "accent": "#ff2a6d", "duration_seconds": 5 },
      "error": { "accent": "#ff5252", "duration_seconds": 2 }
    }
  }
}
```

### Acceptance Criteria

- [ ] Mood-based themes disabled by default
- [ ] When enabled, theme shifts based on time of day
- [ ] Each theme can define its own mood schedule
- [ ] Smooth CSS transitions between moods
- [ ] Can manually lock to a specific mood
- [ ] Event triggers (build success/failure) cause brief accent pulse
- [ ] Mood indicator visible in status bar

---

## Feature 18 — ASCII Art Dashboard

### Description
A startup dashboard that shows: system stats, git status across repos, today's weather, and a rotating ASCII art piece. The cyberpunk version of a "good morning" screen. Changes based on the current theme.

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| AD-01 | Dashboard displays on terminal startup (configurable: enable/disable) | Must |
| AD-02 | System stats: CPU, RAM, disk, uptime | Must |
| AD-03 | Git status: current branch, dirty/clean, ahead/behind for active repos | Must |
| AD-04 | Rotating ASCII art (changes daily or on each launch) | Must |
| AD-05 | ASCII art matches current theme colors | Must |
| AD-06 | Different ASCII art per theme (cyberpunk has neon art, default has simple art) | Must |
| AD-07 | Weather display (via API or cached) | Should |
| AD-08 | Calendar: today's date, day of week | Must |
| AD-09 | Motivational quote or dev tip (rotating) | Could |
| AD-10 | Quick links: recent projects, last commands | Should |
| AD-11 | Customizable layout (which sections to show/hide) | Should |
| AD-12 | ASCII art library in `~/.synthshell/dashboard/art/` | Must |
| AD-13 | User can add custom ASCII art files | Must |
| AD-14 | Dashboard does not slow startup (< 500ms render time) | Must |
| AD-15 | Skip dashboard with hotkey or `--no-dashboard` flag | Must |
| AD-16 | Dashboard art rotates through collection on each launch | Should |

### Dashboard Layout

```
╔══════════════════════════════════════════════════════════════╗
║                                                              ║
║     ___    __    __  .______    __    __   __   __   _______ ║
║    /   \  |  |  |  | |   _  \  |  |  |  | |  | |  | |   ____║║
║   /  ^  \ |  |  |  | |  |_)  | |  |  |  | |  | |  | |  |__  ║
║  /  /_\  \|  |  |  | |   _  <  |  |  |  | |  | |  | |   __| ║
║ /  _____  \  `--'  | |  |_)  | |  `--'  | |  `--'  | |  |____║
║/__/     \__\______/  |______/   \______/   \______/  |_______║
║                                                              ║
║  📅 Tuesday, May 20, 2026          ☁️ 28°C, Dhaka            ║
║                                                              ║
║  💻 System                                                   ║
║  CPU: ████████░░ 78%  RAM: ██████░░░░ 12.4/16 GB  Disk: 67% ║
║  Uptime: 3d 14h                                              ║
║                                                              ║
║  📁 Git Repos                                                ║
║  synthshell        main ✓  ↑2  (clean)                            ║
║  hermes-agent dev  ✗  ↓1  (3 modified)                       ║
║  research     main ✓      (clean)                            ║
║                                                              ║
║  💡 "The best error message is the one that never shows up." ║
║                                                              ║
║  [1] Recent: ~/projects/synthshell  [2] ~/workspace/hermes        ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

### Acceptance Criteria

- [ ] Dashboard renders on terminal startup
- [ ] System stats (CPU, RAM, disk) display correctly
- [ ] Git status shows for configured repos
- [ ] ASCII art matches current theme
- [ ] Different art per theme (cyberpunk art for cyberpunk theme)
- [ ] Dashboard renders in < 500ms
- [ ] Can disable dashboard in settings
- [ ] Can skip with `--no-dashboard` flag
- [ ] User can add custom ASCII art files

---

## Non-Functional Requirements

| ID | Requirement | Details |
|---|---|---|
| NFR-01 | **Low RAM** | Target < 150MB idle, < 300MB with all features active. Tabby baseline is ~200MB — SynthShell must not exceed this. |
| NFR-02 | **Security** | No plaintext secrets. API keys encrypted at rest. Remote access authenticated. No open ports without auth. |
| NFR-03 | **Performance** | Shell switch < 100ms. Theme switch < 500ms. Command palette < 200ms to open. |
| NFR-04 | **Startup** | Cold start < 3 seconds. |
| NFR-05 | **Stability** | No crashes on long-running sessions (24h+). Graceful handling of shell crashes. |
| NFR-06 | **Windows Native** | First-class Windows support. No WSL dependency for the terminal itself. |
| NFR-07 | **Accessibility** | High contrast mode. Font scaling. Screen reader compatible. |

---

## Development Rules

| Rule | Details |
|---|---|
| **Package Manager** | `uv` only. Never `pip`. `uv` is 10-100x faster, resolves dependencies deterministically, produces a lockfile (`uv.lock`), and handles virtual environments automatically. Using `pip` in a `uv` project bypasses the lockfile and creates conflicts. |
| **Package Manager (Node)** | `npm` or `pnpm` for Node/TypeScript dependencies. |
| **Language** | TypeScript (terminal core, plugins), Python (AI integration, tooling), CSS (themes) |
| **Platform** | Windows only for v1. Cross-platform later. |
| **GitHub Sync** | Push to GitHub after every completed stage. Tag releases at milestones. |
| **.gitignore First** | Create `.gitignore` before the first commit. Include `.venv/`, `__pycache__/`, `*.pyc`, `.env`, `node_modules/`, `dist/`, `*.log`. Once files are tracked, `.gitignore` won't untrack them. |
| **Stumbling Block Log** | Every error, blocker, and workaround documented in `STUMBLING-BLOCKS.md`. Include: what failed, why, how it was fixed, and a reusable rule. |
| **Code Style** | ESLint + Prettier for TypeScript. Consistent naming conventions. |
| **Modular Architecture** | Each feature (theme manager, command manager, cache guardian, AI sidebar) is an independent module with a clear input/output interface. Testable in isolation, swappable without touching other modules. |
| **Testing** | Test imports before testing functionality (`python -c "from module import Class; print('OK')"` or equivalent). Unit tests for command manager, theme loader, and AI integration. E2E tests for shell switching and remote control. |
| **Windows Encoding** | Set `PYTHONIOENCODING=utf-8` when spawning Python subprocesses from the terminal. Windows PowerShell defaults to cp1252, which crashes on unicode characters. |
| **Venv Recovery** | If `.venv` breaks on Windows, delete and recreate (`Remove-Item -LiteralPath .venv -Recurse -Force; uv sync`). Don't try to fix it in-place — Windows file locks and symlink issues make manual repair unreliable. |
| **Documentation** | Every feature documented in `/docs`. README updated at each milestone. |

---

## Building in Public — Twitter Dev Log

### Strategy
SynthShell is built in public on X/Twitter. Each milestone produces one Twitter thread documenting the build. The coding agent writes a structured technical summary at the end of each phase — no extra files, just the summary. A separate AI rewrites it into an engaging, human thread optimized for reach.

### Pipeline

```
Coding Agent (Phase Complete)
       │
       ▼
Structured Summary (template below)
       │
       ▼
Second AI (rewrite for Twitter)
       │
       ▼
Thread posted to X/Twitter
```

### Agent Output Template

At the end of each phase, the coding agent MUST produce a summary in this exact format:

```markdown
## Stage: [Phase Name] — [Tag]
- **What was built:** [1-2 sentences, plain language]
- **Key decisions:** [what was chosen and why, 2-3 bullets]
- **Metrics:** [lines of code, features added, bugs fixed, files changed]
- **Stumbling blocks:** [what broke, how it was fixed — keep it real]
- **Vibe:** [one sentence on how it felt — this is the human element]
```

### Rules

| Rule | Details |
|---|---|
| **Frequency** | One thread per milestone (M0-M6). 8 threads total. Don't tweet every commit. |
| **Format** | Each milestone = one thread. First tweet hooks, middle tweets break down, last tweet teases next phase. |
| **Tone** | Technical but human. Show the struggle, not just the win. |
| **Content** | Include real code snippets, screenshots, and metrics. Numbers hook people. |
| **Hook formula** | "I'm building a cyberpunk terminal from scratch. [Phase] just dropped. Here's what happened." |
| **Vibe line** | The coding agent's "Vibe" line is the most important output. It's what makes the tweet human. |

### Twitter Thread Templates

#### M0 — Setup (v0.0.0-setup)
```
Hook: "I'm building SynthShell — a cyberpunk terminal with AI, remote phone control, and themes that look like they belong in Night City. Here's how it started."

Body:
- Forked Tabby (40k+ stars), renamed it, set up CI
- Windows-only, uv-only, never pip
- First commit: .gitignore, build verified, GitHub Actions running
- [Stumbling block if any]

Closer: "Phase 1: Multi-shell support. PowerShell, Bash, Conda — all in one window. Coming next."
```

#### M1 — Multi-Shell (v0.1.0-shell-support)
```
Hook: "SynthShell Phase 1 done. I can now run PowerShell, Bash, and Conda side by side in split panes. Here's the tricky part."

Body:
- Shell auto-detection (PS5, PS7, CMD, Git Bash, Conda)
- Split panes with independent shells
- Conda activation in both PowerShell and Bash
- [Stumbling block if any]

Closer: "Phase 2: The theme library. Cyberpunk Neon with CRT scanlines and particle animations."
```

#### M2 — Theme Library (v0.2.0-theme-library)
```
Hook: "SynthShell Phase 2: Themes. Drop a folder, get a new look. Cyberpunk Neon has CRT scanlines and floating particles."

Body:
- Folder-based theme system (JSON palette + CSS skin + JS effects)
- Cyberpunk Neon flagship theme with scanlines, glow, and particles
- Hot-reload: edit CSS, see changes instantly
- [Stumbling block if any]

Closer: "Phase 3: Command manager. Save commands per-project, recall them instantly."
```

#### M3 — Command Manager (v0.3.0-command-manager)
```
Hook: "SynthShell Phase 3: I got tired of retyping the same commands. So I built a command manager."

Body:
- Global + per-project saved commands
- Ctrl+Shift+P command palette with fuzzy search
- Placeholders: {file}, {branch}, {env} prompt before execution
- [Stumbling block if any]

Closer: "Phase 4: Groq AI sidebar. Ask questions, get commands, insert directly."
```

#### M4 — AI Integration (v0.4.0-ai-integration)
```
Hook: "SynthShell Phase 4: I put an AI inside the terminal. Not a chatbot — a quick-draw assistant."

Body:
- Groq API for sub-second responses
- Context-aware: references current terminal output
- Command suggestions: AI suggests, you click, it executes
- [Stumbling block if any]

Closer: "Phase 5: Remote control. Send commands to your terminal from your phone. From anywhere."
```

#### M5 — Remote Control (v0.5.0-remote-control)
```
Hook: "SynthShell Phase 5: I can now control my terminal from my phone. From a different country."

Body:
- Same WiFi: auto-discovery via mDNS, instant connection
- Anywhere: Tailscale mesh VPN, works through NAT
- Phone WebUI with live terminal output
- Security: auth tokens, kill switch, rate limiting
- [Stumbling block if any]

Closer: "Phase 5.5: Cache Guardian. Your dev tools are hoarding gigabytes. Time to clean house."
```

#### M5.5 — Cache Guardian (v0.5.5-cache-guardian)
```
Hook: "SynthShell Phase 5.5: I had 40GB of cache I didn't know about. So I built Cache Guardian."

Body:
- Scans uv, pip, npm, cargo, go, browsers, IDEs — 15+ caches
- Threshold sliders: green → yellow → red
- Auto-clean when limits exceeded
- [Stumbling block if any]

Closer: "Phase 6: Polish, packaging, and release. SynthShell v1.0 is coming."
```

#### M5.6 — Developer Experience (v0.5.6-dev-experience)
```
Hook: "SynthShell Phase 5.6: Images in the terminal. Git panel. Command history that actually finds things. Five features, one update."

Body:
- Inline image rendering (Kitty protocol + Sixel fallback)
- Git sidebar: branch, diff, one-click commit
- Smart history: fuzzy search with timestamps and directory context
- Session persistence: crash recovery that restores everything
- Markdown rendering: view READMEs with syntax highlighting
- [Stumbling block if any]

Closer: "Phase 6: Polish, packaging, and release. The final stretch."
```

#### M5.7 — Productivity (v0.5.7-productivity)
```
Hook: "SynthShell Phase 5.7: I was drowning in API keys in notepad files. So I built an env var manager that encrypts, copies, and auto-loads per project."

Body:
- Environment Variable Manager: global + per-project, encrypted, one-click copy
- SSH Manager: saved profiles, auto-connect, server status
- Process Monitor: CPU/RAM in real time, kill with one click
- Sound Effects: chime on success, buzz on error, mute hotkey
- [Stumbling block if any]

Closer: "Phase 6: Polish, packaging, and release. The final stretch."
```

#### M5.8 — Smart Features (v0.5.8-smart-features)
```
Hook: "SynthShell Phase 5.8: I can now type 'find large files' in plain English and the terminal translates it to a command. Also, the terminal changes color based on the time of day."

Body:
- Natural Language Command Bar: type what you want, Groq translates to shell commands
- Mood-Based Themes: morning calm → afternoon neutral → night neon
- ASCII Art Dashboard: system stats, git status, and a rotating art piece on startup
- Dashboard art changes per theme (cyberpunk gets neon art, default gets minimal)
- [Stumbling block if any]

Closer: "Phase 6: Polish, packaging, and release. The final stretch."
```

#### M6 — Release (v1.0.0-release)
```
Hook: "SynthShell v1.0.0 is here. A cyberpunk terminal with AI, phone remote control, themes, and cache management. Built in public from scratch."

Body:
- Full feature recap with screenshots
- Performance: [RAM usage], [startup time]
- What I learned building this
- What's next: cross-platform, community themes, plugin ecosystem

Closer: "Star it. Fork it. Build your own theme. The repo: [link]"
```

### Requirements

| ID | Requirement | Priority |
|---|---|---|
| TW-01 | Coding agent outputs structured summary at end of each phase | Must |
| TW-02 | Summary uses the template format (what, decisions, metrics, blocks, vibe) | Must |
| TW-03 | Second AI rewrites summary into Twitter thread | Must |
| TW-04 | One thread per milestone, not per commit | Must |
| TW-05 | Include real metrics (lines, features, bugs fixed) | Should |
| TW-06 | Include screenshots/code snippets in threads | Should |
| TW-07 | Last tweet of each thread teases the next phase | Must |
| TW-08 | Thread templates stored in this document for reference | Must |

---

## Work Breakdown Structure

### Phase 0 — Project Setup

- [ ] 0.1 Repository Setup
  - [ ] 0.1.1 Fork Tabby to GitHub org/personal account
  - [ ] 0.1.2 Rename project to "SynthShell"
  - [ ] 0.1.3 Update package.json, README, LICENSE
  - [ ] 0.1.4 Create `.gitignore` BEFORE first commit (`.venv/`, `__pycache__/`, `*.pyc`, `.env`, `node_modules/`, `dist/`, `*.log`, `*.cache`)
  - [ ] 0.1.5 Configure GitHub Actions for CI (lint, test, build)
  - [ ] 0.1.6 Set up branch strategy (main, dev, feature/*)
  - [ ] 0.1.7 Initial GitHub sync
- [ ] 0.2 Development Environment
  - [ ] 0.2.1 Install uv for Python dependency management
  - [ ] 0.2.2 Configure Node.js environment (nvm or volta)
  - [ ] 0.2.3 Verify Tabby fork builds and runs on Windows
  - [ ] 0.2.4 Document build process in CONTRIBUTING.md
  - [ ] 0.2.5 Set up debugging configuration (VS Code launch.json)

### Phase 1 — Multi-Shell Foundation

- [ ] 1.1 Shell Detection
  - [ ] 1.1.1 Scan system for installed shells (PowerShell, CMD, Git Bash, Conda)
  - [ ] 1.1.2 Detect PowerShell versions (5 vs 7)
  - [ ] 1.1.3 Detect Miniconda/Anaconda installation and environments
  - [ ] 1.1.4 Detect WSL distributions
  - [ ] 1.1.5 Store detected shells in user config
- [ ] 1.2 Shell Profiles
  - [ ] 1.2.1 Create default profile for each detected shell
  - [ ] 1.2.2 Shell profile editor (name, path, args, env vars, working dir)
  - [ ] 1.2.3 Shell-specific icon assignment
  - [ ] 1.2.4 Profile import/export
- [ ] 1.3 Shell Switching
  - [ ] 1.3.1 Dropdown selector per tab
  - [ ] 1.3.2 Hotkey to cycle shells
  - [ ] 1.3.3 Working directory preservation on switch
  - [ ] 1.3.4 Environment variable isolation per shell session
- [ ] 1.4 Conda Integration
  - [ ] 1.4.1 Conda environment listing
  - [ ] 1.4.2 Conda environment selector in dropdown
  - [ ] 1.4.3 Auto-activate base environment on Conda shell open
  - [ ] 1.4.4 Test conda activate/deactivate in PowerShell and Bash
- [ ] 1.5 Split Panes
  - [ ] 1.5.1 Horizontal and vertical split
  - [ ] 1.5.2 Each pane independent shell
  - [ ] 1.5.3 Pane resize with mouse/keyboard
  - [ ] 1.5.4 Pane focus cycling hotkey
- [ ] 1.6 Verification & GitHub Sync
  - [ ] 1.6.1 Test all shells open and execute commands
  - [ ] 1.6.2 Test split panes with different shells
  - [ ] 1.6.3 Document stumbling blocks
  - [ ] 1.6.4 GitHub sync — tag v0.1.0-shell-support

### Phase 2 — Theme Library System

- [ ] 2.1 Theme Manager Core
  - [ ] 2.1.1 Design theme.json schema
  - [ ] 2.1.2 Implement theme directory scanner
  - [ ] 2.1.3 Implement theme loader (read JSON, inject CSS, load JS)
  - [ ] 2.1.4 Theme validator (check required fields, valid colors)
  - [ ] 2.1.5 Theme switcher UI in settings
  - [ ] 2.1.6 Persist selected theme in user config
- [ ] 2.2 xterm.js Theme Integration
  - [ ] 2.2.1 Map theme.json palette to xterm.js theme object
  - [ ] 2.2.2 Apply ANSI 16 colors
  - [ ] 2.2.3 Apply cursor, selection, and background colors
  - [ ] 2.2.4 Font family and size from theme.json
- [ ] 2.3 Effects Layer
  - [ ] 2.3.1 CSS overlay system (append/remove on theme switch)
  - [ ] 2.3.2 CRT scanlines effect (CSS repeating gradient)
  - [ ] 2.3.3 Neon glow effect (CSS text-shadow + box-shadow)
  - [ ] 2.3.4 Canvas overlay for particle animations
  - [ ] 2.3.5 Performance guard: disable effects if CPU > threshold
  - [ ] 2.3.6 Effects toggle in settings
- [ ] 2.4 Cyberpunk Neon Theme (Flagship)
  - [ ] 2.4.1 Design color palette (16 ANSI + bg/fg/cursor/selection)
  - [ ] 2.4.2 Write skin.css (tabs, sidebar, scrollbar, borders)
  - [ ] 2.4.3 Write effects.css (scanlines, glow)
  - [ ] 2.4.4 Write effects.js (particle animation on Canvas)
  - [ ] 2.4.5 Create preview.png screenshot
  - [ ] 2.4.6 Write theme.json with all metadata
- [ ] 2.5 Default Theme
  - [ ] 2.5.1 Create _default theme (no effects, clean look)
  - [ ] 2.5.2 Ensure fallback if selected theme is missing/corrupt
- [ ] 2.6 Theme Template
  - [ ] 2.6.1 Create TEMPLATE theme with comments explaining each file
  - [ ] 2.6.2 Document how to create a new theme (THEME-AUTHORING.md)
- [ ] 2.7 Verification & GitHub Sync
  - [ ] 2.7.1 Test: drop new theme folder, appears in selector
  - [ ] 2.7.2 Test: switch themes, UI updates instantly
  - [ ] 2.7.3 Test: effects load/unload correctly
  - [ ] 2.7.4 Test: corrupt theme falls back to default
  - [ ] 2.7.5 Document stumbling blocks
  - [ ] 2.7.6 GitHub sync — tag v0.2.0-theme-library

### Phase 3 — Command Management

- [ ] 3.1 Command Storage
  - [ ] 3.1.1 Design command schema (JSON)
  - [ ] 3.1.2 Global command store (`~/.synthshell/commands/global.json`)
  - [ ] 3.1.3 Per-project command store (`.synthshell/commands.json`)
  - [ ] 3.1.4 Project detection (`.synthshell/`, `.git/`, `package.json`, `pyproject.toml`, etc.)
  - [ ] 3.1.5 Command CRUD operations (add, edit, delete, duplicate)
- [ ] 3.2 Command Palette
  - [ ] 3.2.1 Ctrl+Shift+P opens command palette
  - [ ] 3.2.2 Fuzzy search across command names, descriptions, tags
  - [ ] 3.2.3 Show global + project commands merged
  - [ ] 3.2.4 Group display (Build, Deploy, Test, etc.)
  - [ ] 3.2.5 Execute command on selection
- [ ] 3.3 Placeholders
  - [ ] 3.3.1 Parse placeholders in command strings (`{file}`, `{branch}`, etc.)
  - [ ] 3.3.2 Prompt user for placeholder values before execution
  - [ ] 3.3.3 Support common placeholders: `{file}`, `{branch}`, `{env}`, `{port}`
  - [ ] 3.3.4 Custom placeholder definitions per command
- [ ] 3.4 Command Sidebar
  - [ ] 3.4.1 Collapsible sidebar with saved commands
  - [ ] 3.4.2 Click to execute
  - [ ] 3.4.3 Drag to reorder
  - [ ] 3.4.4 Right-click context menu (edit, delete, duplicate)
- [ ] 3.5 Import/Export
  - [ ] 3.5.1 Export commands as JSON file
  - [ ] 3.5.2 Import commands from JSON file
  - [ ] 3.5.3 Merge strategy (overwrite, skip duplicates, replace all)
- [ ] 3.6 Verification & GitHub Sync
  - [ ] 3.6.1 Test: add global command, available in any project
  - [ ] 3.6.2 Test: add project command, only available in that project
  - [ ] 3.6.3 Test: fuzzy search finds commands by name and tag
  - [ ] 3.6.4 Test: placeholders prompt correctly
  - [ ] 3.6.5 Document stumbling blocks
  - [ ] 3.6.6 GitHub sync — tag v0.3.0-command-manager

### Phase 4 — AI Integration

- [ ] 4.1 Groq API Setup
  - [ ] 4.1.1 Install groq SDK via npm (TypeScript)
  - [ ] 4.1.2 API key input in settings (masked input)
  - [ ] 4.1.3 Encrypted storage of API key (keytar or OS keychain)
  - [ ] 4.1.4 Connection test button (verify API key works)
  - [ ] 4.1.5 Model selector (llama-3.3-70b, mixtral-8x7b, gemma2-9b-it)
- [ ] 4.2 AI Sidebar
  - [ ] 4.2.1 Toggle sidebar with hotkey (Ctrl+Shift+A)
  - [ ] 4.2.2 Chat input at bottom, responses above
  - [ ] 4.2.3 Conversation history per session
  - [ ] 4.2.4 Clear conversation button
  - [ ] 4.2.5 Copy response to clipboard
- [ ] 4.3 Context Awareness
  - [ ] 4.3.1 Capture last N lines of terminal output as context
  - [ ] 4.3.2 "Ask about this" — send selected terminal text to AI
  - [ ] 4.3.3 Include current shell and working directory in context
- [ ] 4.4 Command Suggestions
  - [ ] 4.4.1 AI suggests commands in response
  - [ ] 4.4.2 "Insert command" button on AI-suggested commands
  - [ ] 4.4.3 "Execute command" button (with confirmation)
  - [ ] 4.4.4 Save AI-suggested command to command manager
- [ ] 4.5 Verification & GitHub Sync
  - [ ] 4.5.1 Test: API key stored securely (not in plaintext)
  - [ ] 4.5.2 Test: AI responds to questions
  - [ ] 4.5.3 Test: command suggestion inserts into terminal
  - [ ] 4.5.4 Test: context awareness references terminal output
  - [ ] 4.5.5 Document stumbling blocks
  - [ ] 4.5.6 GitHub sync — tag v0.4.0-ai-integration

### Phase 5 — Remote Control

- [ ] 5.1 WebSocket Server
  - [ ] 5.1.1 Start WebSocket server on configurable port
  - [ ] 5.1.2 Handle multiple simultaneous connections
  - [ ] 5.1.3 Message protocol (command send, output stream, status, auth)
  - [ ] 5.1.4 Graceful shutdown and restart
- [ ] 5.2 Discovery & Connection
  - [ ] 5.2.1 mDNS broadcast for same-WiFi discovery
  - [ ] 5.2.2 Tailscale IP detection and listener
  - [ ] 5.2.3 Connection priority: local IP → Tailscale
  - [ ] 5.2.4 QR code generation (contains IP + auth token)
  - [ ] 5.2.5 Connection status indicator on PC
- [ ] 5.3 Authentication
  - [ ] 5.3.1 Generate session token on server start
  - [ ] 5.3.2 Token exchange on WebSocket handshake
  - [ ] 5.3.3 Tailscale identity verification
  - [ ] 5.3.4 Rate limiting (max N commands per minute)
  - [ ] 5.3.5 Connection logging (IP, timestamp, commands)
- [ ] 5.4 Phone WebUI
  - [ ] 5.4.1 Mobile-responsive layout (PWA or plain HTML)
  - [ ] 5.4.2 Command input bar
  - [ ] 5.4.3 Live terminal output stream
  - [ ] 5.4.4 Shell selector
  - [ ] 5.4.5 Saved commands list (from command manager)
  - [ ] 5.4.6 Cyberpunk theme matching terminal aesthetic
  - [ ] 5.4.7 Dark mode (default) and light mode toggle
- [ ] 5.5 Security Controls
  - [ ] 5.5.1 Kill switch — disable remote access from PC (hotkey + UI button)
  - [ ] 5.5.2 Approval mode — require PC confirmation before remote command executes
  - [ ] 5.5.3 Session timeout (auto-disconnect after N minutes idle)
  - [ ] 5.5.4 IP whitelist (optional)
  - [ ] 5.5.5 Audit log (who, when, what, from where)
- [ ] 5.6 Verification & GitHub Sync
  - [ ] 5.6.1 Test: same-WiFi connection via QR code
  - [ ] 5.6.2 Test: Tailscale connection from different network
  - [ ] 5.6.3 Test: command execution from phone
  - [ ] 5.6.4 Test: live output streaming to phone
  - [ ] 5.6.5 Test: kill switch disables access
  - [ ] 5.6.6 Test: auth rejects unauthorized connections
  - [ ] 5.6.7 Document stumbling blocks
  - [ ] 5.6.8 GitHub sync — tag v0.5.0-remote-control

### Phase 5.5 — Cache Guardian

- [ ] 5.5.1 Cache Registry
  - [ ] 5.5.1.1 Design cache registry JSON schema
  - [ ] 5.5.1.2 Populate default registry with 15+ Windows caches from CrunchyCleaner's path list
  - [ ] 5.5.1.3 Add dev tool caches: uv, pip, npm, yarn, cargo, go, gradle, nuget
  - [ ] 5.5.1.4 Add browser caches: Chrome, Firefox, Edge, Brave, Opera
  - [ ] 5.5.1.5 Add app caches: Discord, Telegram, Spotify, Slack
  - [ ] 5.5.1.6 Add IDE caches: VS Code, JetBrains
  - [ ] 5.5.1.7 Add system caches: Temp folders, NVIDIA shader cache, Windows logs
  - [ ] 5.5.1.8 Platform-aware path resolution (~ → home dir, %LOCALAPPDATA% → env var)
  - [ ] 5.5.1.9 Glob pattern support for wildcard paths (e.g., JetBrains/*/system/caches)
  - [ ] 5.5.1.10 User-extensible registry (load custom entries from ~/.synthshell/cache-registry.json)
- [ ] 5.5.2 Size Scanner
  - [ ] 5.5.2.1 Recursive directory size calculation
  - [ ] 5.5.2.2 Handle permission errors gracefully (skip inaccessible dirs)
  - [ ] 5.5.2.3 Cache results to avoid re-scanning within interval
  - [ ] 5.5.2.4 On-demand scan trigger (manual button)
  - [ ] 5.5.2.5 Periodic background scan (configurable interval, default 6h)
  - [ ] 5.5.2.6 Lazy load — do not scan until panel is opened or interval fires
  - [ ] 5.5.2.7 Scan progress indicator for large directories
- [ ] 5.5.3 Threshold System
  - [ ] 5.5.3.1 Default thresholds (yellow: 500MB, red: 2000MB)
  - [ ] 5.5.3.2 Per-cache threshold overrides in config
  - [ ] 5.5.3.3 Category-level thresholds (all Package Managers share a limit)
  - [ ] 5.5.3.4 Threshold comparison logic (green → yellow → red state machine)
  - [ ] 5.5.3.5 State persistence across sessions
- [ ] 5.5.4 UI — Cache Guardian Panel
  - [ ] 5.5.4.1 Collapsible sidebar panel
  - [ ] 5.5.4.2 Group caches by category (Package Managers, Browsers, IDEs, Apps, System)
  - [ ] 5.5.4.3 Per-cache size bar with color coding (green/yellow/red)
  - [ ] 5.5.4.4 Threshold slider UI (drag to set yellow and red limits)
  - [ ] 5.5.4.5 Total cache size summary at top of panel
  - [ ] 5.5.4.6 One-click clean button per cache entry
  - [ ] 5.5.4.7 "Clean All Red" batch button
  - [ ] 5.5.4.8 "Rescan" button
  - [ ] 5.5.4.9 Settings gear icon (open threshold config)
- [ ] 5.5.5 Notifications
  - [ ] 5.5.5.1 Red banner notification in terminal UI when any cache exceeds red threshold
  - [ ] 5.5.5.2 Windows toast notification on threshold breach
  - [ ] 5.5.5.3 Notification cooldown (don't spam — max once per hour per cache)
  - [ ] 5.5.5.4 Status bar indicator showing number of red caches
- [ ] 5.5.6 Cleaner
  - [ ] 5.5.6.1 Map each cache to its native clean command (uv cache clean, npm cache clean --force, etc.)
  - [ ] 5.5.6.2 Execute native clean commands via child_process
  - [ ] 5.5.6.3 Fallback: direct directory deletion when no native command exists
  - [ ] 5.5.6.4 Dry-run mode — list files that would be deleted without deleting
  - [ ] 5.5.6.5 Cleanup log (timestamp, cache name, size before, size after, bytes freed)
  - [ ] 5.5.6.6 Auto-clean — trigger clean automatically when red threshold is breached
  - [ ] 5.5.6.7 Auto-clean confirmation option (notify + wait for approval vs. silent clean)
  - [ ] 5.5.6.8 Undo safety — move to recycle bin instead of permanent delete (configurable)
- [ ] 5.5.7 Verification & GitHub Sync
  - [ ] 5.5.7.1 Test: scanner correctly calculates sizes for all 15+ caches
  - [ ] 5.5.7.2 Test: threshold slider updates state and persists
  - [ ] 5.5.7.3 Test: red notification triggers on breach
  - [ ] 5.5.7.4 Test: one-click clean uses native command where available
  - [ ] 5.5.7.5 Test: auto-clean triggers within 1 minute of breach
  - [ ] 5.5.7.6 Test: dry-run mode lists files without deleting
  - [ ] 5.5.7.7 Test: cleanup log records correct data
  - [ ] 5.5.7.8 Test: user can add custom cache entry and it appears in panel
  - [ ] 5.5.7.9 Test: background scanner uses < 2% CPU
  - [ ] 5.5.7.10 Document stumbling blocks
  - [ ] 5.5.7.11 GitHub sync — tag v0.5.5-cache-guardian

### Phase 5.6 — Developer Experience Features

- [ ] 5.6.1 Inline Image Rendering (DEFERRED - see late-improvements.md)
  - [ ] 5.6.1.1 Implement Kitty graphics protocol renderer
  - [ ] 5.6.1.2 Implement Sixel format fallback
  - [ ] 5.6.1.3 Auto-detect terminal capability (Kitty vs Sixel vs none)
  - [ ] 5.6.1.4 Image scaling to fit pane width/height
  - [ ] 5.6.1.5 Configurable max image size limits
  - [ ] 5.6.1.6 Support PNG, JPEG, GIF (static), WebP
  - [ ] 5.6.1.7 Non-blocking rendering (terminal input stays responsive)
  - [ ] 5.6.1.8 Image cleanup on clear/scroll
  - [ ] 5.6.1.9 Split pane awareness
  - [ ] 5.6.1.10 Test with various image sizes and formats
- [ ] 5.6.2 Git Integration Panel
  - [ ] 5.6.2.1 Auto-detect git repos from cwd
  - [ ] 5.6.2.2 Display current branch name in panel header
  - [ ] 5.6.2.3 List changed/staged/untracked files with status icons
  - [ ] 5.6.2.4 Color-coded file status (green/red/grey)
  - [ ] 5.6.2.5 Click file to preview diff in split view
  - [ ] 5.6.2.6 One-click stage/unstage individual files
  - [ ] 5.6.2.7 Commit message input with one-click commit
  - [ ] 5.6.2.8 Git status summary (modified, new, untracked counts)
  - [ ] 5.6.2.9 Ahead/behind remote indicator
  - [ ] 5.6.2.10 Branch switcher dropdown
  - [ ] 5.6.2.11 Collapsible panel with hotkey toggle
  - [ ] 5.6.2.12 Performance: < 100ms refresh in large repos
  - [ ] 5.6.2.13 Test in repos with 1000+ files
- [ ] 5.6.3 Smart History Search (DEFERRED - see late-improvements.md)
  - [ ] 5.6.3.1 Fuzzy search engine (Ctrl+R replacement)
  - [ ] 5.6.3.2 Record timestamp and working directory per command
  - [ ] 5.6.3.3 Record shell type per command
  - [ ] 5.6.3.4 Search results with timestamp, directory, and shell display
  - [ ] 5.6.3.5 Preview pane for command output
  - [ ] 5.6.3.6 Filter by date range, directory, shell type
  - [ ] 5.6.3.7 Relevance + recency sorting algorithm
  - [ ] 5.6.3.8 Insert selected command without executing
  - [ ] 5.6.3.9 SQLite backend for fast queries on large history
  - [ ] 5.6.3.10 Cross-session history aggregation
  - [ ] 5.6.3.11 Optional: sync history across machines via Tailscale
  - [ ] 5.6.3.12 Test with 100k+ history entries
- [ ] 5.6.4 Session Persistence
  - [ ] 5.6.4.1 Session state schema (JSON: tabs, panes, shells, dirs, focus)
  - [ ] 5.6.4.2 Auto-save on interval (configurable, default 30s)
  - [ ] 5.6.4.3 Save on graceful close
  - [ ] 5.6.4.4 Restore all tabs on reopen
  - [ ] 5.6.4.5 Restore all panes within each tab
  - [ ] 5.6.4.6 Restore shell type and working directory per tab/pane
  - [ ] 5.6.4.7 Restore active tab/pane focus
  - [ ] 5.6.4.8 Restore scrollback buffer (configurable depth)
  - [ ] 5.6.4.9 Crash recovery: detect unclean shutdown, offer restore
  - [ ] 5.6.4.10 Session state storage in `~/.synthshell/sessions/`
  - [ ] 5.6.4.11 Encrypt sensitive env vars in session state
  - [ ] 5.6.4.12 Multiple session profiles (Work, Personal, Project X)
  - [ ] 5.6.4.13 Test: crash recovery with 10 tabs and 5 panes
- [ ] 5.6.5 Inline Markdown Rendering
  - [ ] 5.6.5.1 Markdown parser (headers, bold, italic, strikethrough)
  - [ ] 5.6.5.2 Code block renderer with syntax highlighting
  - [ ] 5.6.5.3 Table renderer with aligned columns
  - [ ] 5.6.5.4 List renderer (ordered + unordered)
  - [ ] 5.6.5.5 Inline code with background highlight
  - [ ] 5.6.5.6 Blockquote with left-border styling
  - [ ] 5.6.5.7 Clickable links (OSC 8) or styled text fallback
  - [ ] 5.6.5.8 `view` command to render markdown files
  - [ ] 5.6.5.9 Pipe support (`cat README.md | synthshell-md`)
  - [ ] 5.6.5.10 Syntax highlighting uses current theme palette
  - [ ] 5.6.5.11 Paginated output for large files
  - [ ] 5.6.5.12 `cat` still shows raw markdown (rendering is opt-in)
  - [ ] 5.6.5.13 Test with READMEs, docs, and large markdown files
- [ ] 5.6.6 Verification & GitHub Sync
  - [ ] 5.6.6.1 Test: inline image rendering across formats
  - [ ] 5.6.6.2 Test: git panel in repos of varying sizes
  - [ ] 5.6.6.3 Test: smart history search with 100k+ entries
  - [ ] 5.6.6.4 Test: session persistence survives crash simulation
  - [ ] 5.6.6.5 Test: markdown rendering with complex documents
  - [ ] 5.6.6.6 Document stumbling blocks
  - [ ] 5.6.6.7 GitHub sync — tag v0.5.6-dev-experience

### Phase 5.7 — Productivity Features

- [ ] 5.7.1 SSH Manager
  - [ ] 5.7.1.1 SSH profile data model (host, port, user, key, label, group)
  - [ ] 5.7.1.2 Profile storage in `~/.synthshell/ssh/profiles.json` (encrypted)
  - [ ] 5.7.1.3 Import from `~/.ssh/config`
  - [ ] 5.7.1.4 Profile groups (Production, Staging, Personal)
  - [ ] 5.7.1.5 Quick-connect via profile name or hotkey
  - [ ] 5.7.1.6 Server status indicator (ping check on interval)
  - [ ] 5.7.1.7 Connection history tracking
  - [ ] 5.7.1.8 Auto-reconnect on disconnect
  - [ ] 5.7.1.9 Sidebar with search/filter
  - [ ] 5.7.1.10 Export profiles to JSON
  - [ ] 5.7.1.11 Test with 20+ profiles
- [ ] 5.7.2 Environment Variable Manager
  - [ ] 5.7.2.1 Env var storage schema (global + per-project, encrypted)
  - [ ] 5.7.2.2 Storage in `~/.synthshell/env/` with AES encryption
  - [ ] 5.7.2.3 Global env var injection into all new shell sessions
  - [ ] 5.7.2.4 Per-project env var auto-loading (detect project from cwd)
  - [ ] 5.7.2.5 Project detection (`.synthshell/env.json`, `.env`, `.git/`, `pyproject.toml`)
  - [ ] 5.7.2.6 UI panel: add/edit/delete env vars
  - [ ] 5.7.2.7 Masked display for sensitive values (`sk-or-...7df8`)
  - [ ] 5.7.2.8 Toggle reveal/hide per value
  - [ ] 5.7.2.9 Copy value to clipboard (one click)
  - [ ] 5.7.2.10 Copy as `export KEY=value` (Bash format)
  - [ ] 5.7.2.11 Copy as `$env:KEY="value"` (PowerShell format)
  - [ ] 5.7.2.12 Copy full `.env` file content to clipboard
  - [ ] 5.7.2.13 Import from existing `.env` files
  - [ ] 5.7.2.14 Export as `.env` file for a project
  - [ ] 5.7.2.15 Tags for categorization (AI, Cloud, Database, Dev)
  - [ ] 5.7.2.16 Search/filter by name or tag
  - [ ] 5.7.2.17 Quick-paste: paste API key, auto-detect var name
  - [ ] 5.7.2.18 Bulk import from pasted `KEY=value` block
  - [ ] 5.7.2.19 Env vars accessible from phone WebUI
  - [ ] 5.7.2.20 Collapsible sidebar panel with category groups
  - [ ] 5.7.2.21 Test: global vars available in PowerShell, Bash, CMD
  - [ ] 5.7.2.22 Test: per-project vars auto-load on directory change
  - [ ] 5.7.2.23 Test: copy-paste into external project works
- [ ] 5.7.3 Process Monitor (DEFERRED - see late-improvements.md)
  - [ ] 5.7.3.1 CPU usage display (overall percentage)
  - [ ] 5.7.3.2 RAM usage display (used/total, percentage)
  - [ ] 5.7.3.3 Disk usage for primary drive
  - [ ] 5.7.3.4 Top 10 processes by CPU or RAM
  - [ ] 5.7.3.5 Process name, PID, CPU%, RAM% per entry
  - [ ] 5.7.3.6 Sort toggle (CPU vs RAM)
  - [ ] 5.7.3.7 Kill process with confirmation dialog
  - [ ] 5.7.3.8 Auto-refresh (configurable, default 2s)
  - [ ] 5.7.3.9 Pause/resume refresh
  - [ ] 5.7.3.10 Color-coded thresholds (green/yellow/red)
  - [ ] 5.7.3.11 Collapsible panel with hotkey
  - [ ] 5.7.3.12 Verify monitor uses < 1% CPU
- [ ] 5.7.4 Custom Sound Effects
  - [ ] 5.7.4.1 Sound event system (success, error, build, notification, alert, connect)
  - [ ] 5.7.4.2 Per-event enable/disable toggle
  - [ ] 5.7.4.3 Volume control (master + per-event)
  - [ ] 5.7.4.4 Built-in sound pack (3-5 options per event)
  - [ ] 5.7.4.5 Custom sound file upload per event (.wav/.mp3)
  - [ ] 5.7.4.6 Mute all hotkey
  - [ ] 5.7.4.7 Quiet hours (configurable time range)
  - [ ] 5.7.4.8 Sound on Cache Guardian threshold breach
  - [ ] 5.7.4.9 Sound on remote phone connection
  - [ ] 5.7.4.10 Opt-in only (no sound by default)
  - [ ] 5.7.4.11 Sound files stored in `~/.synthshell/sounds/`
  - [ ] 5.7.4.12 Windows native audio (no external deps)
- [ ] 5.7.5 Verification & GitHub Sync
  - [ ] 5.7.5.1 Test: SSH profile connect/disconnect
  - [ ] 5.7.5.2 Test: env var injection into all shell types
  - [ ] 5.7.5.3 Test: copy-paste API key into external project
  - [ ] 5.7.5.4 Test: process monitor accuracy
  - [ ] 5.7.5.5 Test: sound events trigger correctly
  - [ ] 5.7.5.6 Document stumbling blocks
  - [ ] 5.7.5.7 GitHub sync — tag v0.5.7-productivity

### Phase 5.8 — Smart Features

- [ ] 5.8.1 Natural Language Command Bar
  - [ ] 5.8.1.1 NL input bar UI (hotkey to open, text input, results display)
  - [ ] 5.8.1.2 Groq API integration for NL → command translation
  - [ ] 5.8.1.3 Shell context injection (PowerShell vs Bash syntax in prompt)
  - [ ] 5.8.1.4 Directory context injection (project type, files present)
  - [ ] 5.8.1.5 Command display with explanation
  - [ ] 5.8.1.6 Run / Copy / Edit buttons
  - [ ] 5.8.1.7 Dangerous command detection and warning
  - [ ] 5.8.1.8 Multi-step command support (pipelines)
  - [ ] 5.8.1.9 Query history
  - [ ] 5.8.1.10 Fallback when Groq unavailable
  - [ ] 5.8.1.11 Test with 20 different natural language queries
- [ ] 5.8.2 Mood-Based Themes
  - [ ] 5.8.2.1 Extend theme.json schema with `mood_schedule` object
  - [ ] 5.8.2.2 Mood scheduler (time-based mood switching)
  - [ ] 5.8.2.3 Default mood schedule (morning/afternoon/evening/night)
  - [ ] 5.8.2.4 User-customizable time ranges per mood
  - [ ] 5.8.2.5 Palette override application per mood
  - [ ] 5.8.2.6 CSS transition for smooth mood shifts
  - [ ] 5.8.2.7 Event triggers (build success/failure, error)
  - [ ] 5.8.2.8 Event accent pulse (brief color flash)
  - [ ] 5.8.2.9 Mood indicator in status bar
  - [ ] 5.8.2.10 Manual mood lock (override to specific mood)
  - [ ] 5.8.2.11 Opt-in toggle (disabled by default)
  - [ ] 5.8.2.12 Add mood_schedule to Cyberpunk Neon theme
  - [ ] 5.8.2.13 Test: mood transitions at different times
- [ ] 5.8.3 ASCII Art Dashboard
  - [ ] 5.8.3.1 Dashboard renderer (startup screen)
  - [ ] 5.8.3.2 System stats collection (CPU, RAM, disk, uptime)
  - [ ] 5.8.3.3 Git status for configured repos
  - [ ] 5.8.3.4 ASCII art library (`~/.synthshell/dashboard/art/`)
  - [ ] 5.8.3.5 Theme-aware art (different art per theme)
  - [ ] 5.8.3.6 Art rotation (daily or per-launch)
  - [ ] 5.8.3.7 Calendar/date display
  - [ ] 5.8.3.8 Weather display (API integration or cached)
  - [ ] 5.8.3.9 Quick links (recent projects, last commands)
  - [ ] 5.8.3.10 Customizable layout (show/hide sections)
  - [ ] 5.8.3.11 Custom ASCII art upload support
  - [ ] 5.8.3.12 `--no-dashboard` flag
  - [ ] 5.8.3.13 Dashboard render < 500ms
  - [ ] 5.8.3.14 Create 3 ASCII art variants (cyberpunk, minimal, retro)
  - [ ] 5.8.3.15 Test: dashboard with different themes
- [ ] 5.8.4 Verification & GitHub Sync
  - [ ] 5.8.4.1 Test: NL command bar translates correctly for 20 queries
  - [ ] 5.8.4.2 Test: mood transitions work at different times
  - [ ] 5.8.4.3 Test: dashboard renders in < 500ms
  - [ ] 5.8.4.4 Test: ASCII art changes per theme
  - [ ] 5.8.4.5 Document stumbling blocks
  - [ ] 5.8.4.6 GitHub sync — tag v0.5.8-smart-features

### Phase 6 — Polish & Packaging

- [ ] 6.1 Performance Optimization
  - [ ] 6.1.1 Profile RAM usage, identify leaks
  - [ ] 6.1.2 Optimize theme effects for low CPU
  - [ ] 6.1.3 Lazy load AI sidebar and remote server
  - [ ] 6.1.4 Target: < 150MB idle, < 300MB fully loaded
- [ ] 6.2 Security Audit
  - [ ] 6.2.1 Verify no plaintext secrets anywhere
  - [ ] 6.2.2 Penetration test remote control (unauthorized access attempt)
  - [ ] 6.2.3 Verify rate limiting and session timeout
  - [ ] 6.2.4 Code review for injection vulnerabilities
- [ ] 6.3 Installer & Distribution
  - [ ] 6.3.1 Build Windows installer (electron-builder or NSIS)
  - [ ] 6.3.2 Portable mode (no install, run from USB)
  - [ ] 6.3.3 Auto-update mechanism
  - [ ] 6.3.4 First-run wizard (shell detection, theme selection, API key)
- [ ] 6.4 Documentation
  - [ ] 6.4.1 README with screenshots and feature overview
  - [ ] 6.4.2 Installation guide
  - [ ] 6.4.3 Theme authoring guide
  - [ ] 6.4.4 Command manager guide
  - [ ] 6.4.5 Remote control setup guide
  - [ ] 6.4.6 Stumbling blocks compilation (all phases)
- [ ] 6.5 Final GitHub Sync
  - [ ] 6.5.1 Tag v1.0.0
  - [ ] 6.5.2 Create GitHub Release with installer
  - [ ] 6.5.3 Publish to relevant communities (Reddit, HN, Dev.to)

---

## Stumbling Block Log

> **Every error, blocker, and workaround MUST be logged here.**
> Format: Date | Block | Root Cause | Solution | Reusable Rule

| Date | Block | Root Cause | Solution | Reusable Rule |
|---|---|---|---|---|
| — | — | — | — | — |

---

## Milestones & GitHub Sync Points

| Milestone | Tag | Phase | Description |
|---|---|---|---|
| M0 | v0.0.0-setup | Phase 0 | Fork renamed, build verified, CI running |
| M1 | v0.1.0-shell-support | Phase 1 | Multi-shell switching and split panes working |
| M2 | v0.2.0-theme-library | Phase 2 | Theme system + Cyberpunk Neon theme live |
| M3 | v0.3.0-command-manager | Phase 3 | Command palette with per-project scoping |
| M4 | v0.4.0-ai-integration | Phase 4 | Groq AI sidebar with command suggestions |
| M5 | v0.5.0-remote-control | Phase 5 | Phone WebUI via WiFi and Tailscale |
| M5.5 | v0.5.5-cache-guardian | Phase 5.5 | Cache monitoring, thresholds, auto-clean |
| M5.6 | v0.5.6-dev-experience | Phase 5.6 | Image rendering, git panel, smart history, session persistence, markdown |
| M5.7 | v0.5.7-productivity | Phase 5.7 | SSH manager, env var manager, process monitor, sound effects |
| M5.8 | v0.5.8-smart-features | Phase 5.8 | NL command bar, mood-based themes, ASCII dashboard |
| M6 | v1.0.0-release | Phase 6 | Polished, packaged, documented, public |

---

## Open Questions

| Question | Status | Owner |
|---|---|---|
| Tabby fork or Electron + xterm.js from scratch? | **Resolved: Fork Tabby** | — |
| Tailscale free tier sufficient? | Open | — |
| Phone WebUI: PWA or plain HTML? | Open | — |
| Groq SDK: Python or TypeScript? | Open | — |
| Electron-builder or NSIS for installer? | Open | — |

---

*This document is the single source of truth for SynthShell. Update it as decisions are made and features evolve.*
