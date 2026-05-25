# SynthShell — AI Terminal Emulator
> A conceptual AI-powered terminal emulator that bridges natural language and system commands.

## Vision

SynthShell reimagines the terminal as an **AI-native interface**. Instead of memorizing command syntax, you describe what you want in natural language and SynthShell translates, explains, and executes.

## Spec Documents

- **[AI Terminal Spec](SYNTHSHELL_AI_TERMINAL_SPEC.md)** — Core architecture, command translation pipeline, plugin system
- **[User Spec](SYNTHSHELL_AI_TERMINAL_USER_SPEC.md)** — User-facing interaction model, safety constraints, configuration

## Key Concepts

| Concept | Description |
|---------|-------------|
| **NL→CMD** | Natural language to shell command translation |
| **Confidence Scoring** | Commands show confidence before execution |
| **Sandbox Mode** | Dry-run / preview before real execution |
| **Plugin System** | Extensible command pipeline hooks |
| **Context Awareness** | Remembers directory, history, project context |

## Status

This is a **concept/spec project**. No implementation code yet — the docs define the architecture for future development.
