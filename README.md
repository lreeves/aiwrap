# aiwrap

Run AI coding agents in a sandboxed environment using [bubblewrap](https://github.com/containers/bubblewrap).

## Prerequisites

- Python 3
- Node.js and npm
- bubblewrap (`bwrap`)

On Arch Linux:

```bash
pacman -S bubblewrap nodejs npm python
```

## Setup

Install the coding tools into a local prefix:

```bash
./enter --setup
```

This runs `npm install -g` for each tool into `~/.local/share/aiwrap/tools/node/`. Run it again to update.

## Usage

`cd` into a project directory, then:

```bash
./enter claude    # Claude Code
./enter codex     # OpenAI Codex
./enter qwen      # Qwen Code
./enter gemini    # Gemini CLI
./enter pi        # pi-coding-agent
./enter bash      # Plain shell
```

The tool runs inside a bwrap sandbox where only the current directory is writable. Environment variables (API keys, etc.) are passed through automatically.

## How the sandbox works

- The current working directory is mounted read-write at its real path
- `/usr`, `/etc`, `/bin`, `/lib` are mounted read-only from the host
- PID, UTS, and cgroup namespaces are isolated
- Network access is allowed (tools need it for API calls)
- `~/.pyenv` and `~/.gitconfig` are mounted read-only if present
- `~/.qwen` and `~/.pi` are mounted writable only for their respective tools
- Everything else is inaccessible
