# check-hostname

An agent skill that answers one question: **where is this AI session actually running?**

Agent sessions often have more than one place to run commands - your real computer, a VM on it, WSL, a container, or a cloud sandbox. A plain `hostname` only tells you about whichever shell happened to answer. This skill makes the agent check all of them and report back in three parts:

```
**Physical:** Windows 11 - OFFICE-PC
**Available:**
- Linux (Ubuntu 24.04) - a1b2c3d4e5f6 (cloud sandbox)
- Linux (Ubuntu 22.04) - agent-vm (VM on OFFICE-PC)
**Chat using:** Linux (Ubuntu 24.04) - a1b2c3d4e5f6 (cloud sandbox)
```

- **Physical** - your real computer's OS and name.
- **Available** - every environment the session can run commands in.
- **Chat using** - the one the session itself is running in.

It works on Windows, macOS and Linux, and never reports a sandbox or VM name as if it were your computer.

## What it runs

Only read-only identification commands:

| OS | Commands |
|---|---|
| Windows | `hostname`, `(Get-CimInstance Win32_OperatingSystem).Caption` |
| macOS | `hostname`, `sw_vers`, `scutil --get ComputerName` |
| Linux | `hostname`, reads `/etc/os-release`, `uname` |

No network access, no installs, no file changes, no elevated permissions. The skill is a single Markdown file with no scripts, so you can read everything it does in `skills/check-hostname/SKILL.md`.

## Install

The skill uses the open [Agent Skills](https://agentskills.io) format (`SKILL.md` with YAML front matter), so it works in any agent that supports that format. Copy the `skills/check-hostname` folder into your agent's skills directory.

```bash
git clone https://github.com/joermwatts/check-hostname-skill.git
```

Then copy `check-hostname-skill/skills/check-hostname` to the right place for your agent. Common locations:

| Agent | Personal skills folder | Project skills folder |
|---|---|---|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| OpenAI Codex CLI | `~/.codex/skills/` | `.agents/skills/` |
| Gemini CLI | `~/.gemini/skills/` | `.gemini/skills/` |
| Cursor | `~/.cursor/skills/` | `.cursor/skills/` |
| GitHub Copilot / VS Code | `~/.copilot/skills/` | `.github/skills/` |
| Other agents | see your agent's docs | often `.agents/skills/` |

Skills folders differ between agents and versions, so check your agent's documentation if the skill does not appear.

For the Claude apps (claude.ai, desktop), zip the `check-hostname` folder and upload it under Settings > Capabilities > Skills.

### Agents without skill support

The skill is plain Markdown instructions, so it still works without a skills system. Either:

- paste the body of `SKILL.md` (everything below the front matter) into your agent's rules or instructions file (`AGENTS.md`, `.cursorrules`, custom instructions and so on), or
- keep this repo in your project and let the agent pick up the included `AGENTS.md`, which points to the skill.

## Use

Ask your agent something like:

- 'Which PC is this chat running on?'
- 'What machine are you on?'
- 'Are you in a sandbox or on my laptop?'

Or invoke it by name, e.g. `/check-hostname` in agents that expose skills as commands.

## Privacy note

The output contains your computer's hostname and OS version. That stays in your conversation with the agent - the skill sends nothing anywhere - but be aware of it before pasting the output somewhere public.

## Repository layout

```
check-hostname-skill/
├── skills/
│   └── check-hostname/
│       └── SKILL.md      # the skill itself
├── AGENTS.md             # pointer for agents without skill support
├── LICENSE               # MIT
└── README.md
```

## Licence

[MIT](LICENSE)
