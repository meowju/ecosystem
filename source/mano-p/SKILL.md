---
origin: local
name: mano-p
description: Desktop GUI automation via natural language. Captures screenshots, sends to vision model, executes actions (click, type, scroll, drag, hotkey) locally. Supports cloud mode (default) and local mode (on-device MLX).
homepage: https://github.com/Mininglamp-AI/mano-skill
license: MIT
---

# Mano-P Skill

Desktop GUI automation driven by natural language. Captures screenshots, sends them to a cloud-based hybrid vision model, and executes returned actions on the local machine.

## Installation

### Option 1: Homebrew (recommended on macOS)

```bash
brew tap Mininglamp-AI/tap
brew install mano-cua
```

### Option 2: OpenClaw / ClawHub

```bash
openclaw skills install mano-cua
# or
clawhub install mano-cua
```

### Option 3: Python directly (development)

```bash
git clone https://github.com/Mininglamp-AI/mano-skill
cd mano-skill
pip install -r requirements.txt
python -m visual.vla run "your task"
```

## Permissions Required

Before first use, grant these in **System Preferences → Privacy & Security**:

1. **Screen Recording** — required for screenshot capture
2. **Accessibility** — required for keyboard/mouse control

Without these, `mano-cua` will fail or produce no-op actions.

## Usage

### Basic (cloud mode — no setup)

```bash
mano-cua run "Open Safari and search for Python"
mano-cua run "Open WeChat and tell FTY that the meeting is postponed"
```

### With URL scoping

```bash
mano-cua run "Search for AI news" --url "https://www.xiaohongshu.com"
```

### Local mode (on-device, Apple Silicon + 32GB RAM)

```bash
mano-cua run "Open Finder and navigate to Documents" --local
```

### Other commands

```bash
mano-cua check          # Verify installation and permissions
mano-cua stop           # Stop active session
mano-cua --help         # Full CLI reference
```

## Supported Actions

`click` · `type` · `hotkey` · `scroll` · `drag` · `mouse move` · `screenshot` · `wait` · `app launch` · `url navigation`

## How It Works

```
User → Local client → Cloud Server → Model selection
              ↓              ↓
         screenshot    ┌───────┴───────┐
                       │               │
                  Mano Model      Claude CUA
                  (fast/repetitive)  (complex reasoning)
```

- **Cloud mode** (default): screenshots sent to `mano.mininglamp.com`. Fast, works on any Mac/PC.
- **Local mode** (`--local`): runs Qwen3-VL via MLX on Apple Silicon. Maximum privacy, requires M4+ chip with 32GB RAM.

## Passing to Subagents (Desktop Task Delegation)

When a subagent needs to perform desktop/GUI tasks, pass the mano-p skill:

### Via coding-agent / spawn

```json
{
  "skill": "mano-p",
  "task": "Open Safari, navigate to github.com, and search for meowju/skills"
}
```

### Within OpenClaw/Claude Code

Use the `summon` tool or session spawn to pass the skill context:

```
summon mano-p --task "Open Finder, create a folder named Projects"
```

### Key constraints when delegating

1. **Grant permissions before delegation** — Screen Recording + Accessibility must be approved in System Preferences
2. **Do not use mouse/keyboard during active task** — manual input can conflict with automated actions
3. **Primary display only** — multi-monitor setups only use the main display
4. **User consent** — `mano-cua` prompts before sensitive steps. Inform the user before delegating destructive tasks
5. **Avoid sensitive apps** — warn user to close apps with sensitive data before running automated tasks
6. **macOS preferred** — Windows/Linux support is Beta quality

## Status Panel

A small overlay appears in the top-right corner showing session status. This is visible to the user, so they knowmano-cua is active.

## Error Handling

- Permission denied → run `mano-cua check` and follow instructions to grant in System Preferences
- No active session → `mano-cua run` is already running, or session expired; try `mano-cua stop` then re-run
- Local mode fails → ensure Apple Silicon Mac with 32GB+ RAM; fall back to cloud mode without `--local`

## Model Info

- **Mano-P 1.0-4B**: ~80 tokens/s on Apple M5 Pro
- **Claude CUA Model**: used for complex multi-step reasoning tasks
- Both routed automatically by the server based on task complexity
- Download models: [Hugging Face](https://huggingface.co/Mininglamp-2718/Mano-P)