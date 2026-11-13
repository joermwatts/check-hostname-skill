---
name: check-hostname
description: Reports where an AI agent session is running - the physical computer's OS and name, every environment the session can run commands in (cloud sandbox, VM, WSL, container) with OS and name, and which one the session is using. Use whenever the user asks which PC, Mac, machine, device or sandbox this is, or before machine-specific work where the wrong machine would matter.
license: MIT
compatibility: Needs a shell or command-execution tool. Works on Windows (PowerShell or cmd), macOS and Linux. No network access, installs or elevated permissions required.
metadata:
  author: joermwatts
  version: "1.0.0"
  tags: hostname, environment, diagnostics, sandbox, cross-platform, windows, macos, linux
---

# Check hostname

People often work across several computers and in cloud sessions, and one agent session can have more than one place to run commands (the user's computer, a VM on it, a cloud sandbox). Before machine-specific work (paths, drives, local services, installed tools) it helps to see all of them at a glance and know which one the session is actually using.

This skill is read-only. It runs a couple of standard identification commands and reports the result. It does not change anything, install anything, or send anything anywhere.

## Output

Reply with exactly these three parts and nothing else:

```
**Physical:** <OS> - <name>
**Available:**
- <OS> - <name> (<what it is>)
- <OS> - <name> (<what it is>)
**Chat using:** <OS> - <name> (<what it is>)
```

Example from a cloud session linked to a Windows PC:

```
**Physical:** Windows 11 - OFFICE-PC
**Available:**
- Linux (Ubuntu 24.04) - a1b2c3d4e5f6 (cloud sandbox)
- Linux (Ubuntu 22.04) - agent-vm (VM on OFFICE-PC)
**Chat using:** Linux (Ubuntu 24.04) - a1b2c3d4e5f6 (cloud sandbox)
```

Example from an agent running directly on the computer:

```
**Physical:** Windows 11 - OFFICE-PC
**Available:**
- Windows 11 - OFFICE-PC (PowerShell on the PC)
**Chat using:** Windows 11 - OFFICE-PC (PowerShell on the PC)
```

What each part means:

- **Physical** - the user's real computer. Only a name that genuinely comes from that machine goes here (see 'Working out what each shell is').
- **Available** - every environment this session can run commands in, one line each. Include the physical machine itself if it has a directly reachable shell.
- **Chat using** - the environment the session itself is running in, i.e. where its default shell and working directory live. It is always one of the Available lines.

If a part cannot be determined, write 'not reachable from this session' or 'none linked' rather than leaving it out or guessing, so the three parts are always present.

## How to gather it

These commands only read the machine's name and OS version, so run them without asking first unless your agent's own rules require confirmation for every command. Run them in every shell or command tool the session has (for example a cloud shell and a device shell), in parallel where possible.

| OS | Shell | Name | OS and version |
|---|---|---|---|
| Windows | PowerShell (preferred) or cmd | `hostname` | `(Get-CimInstance Win32_OperatingSystem).Caption` |
| macOS | zsh / bash | `hostname` | `sw_vers -productName; sw_vers -productVersion` |
| Linux | bash / sh | `hostname` | `. /etc/os-release && echo "$PRETTY_NAME"` |

On Windows, use a PowerShell tool if one is available; if only a generic shell tool is available, wrap the commands as `powershell -NoProfile -Command "..."`.

If the agent has a device-information tool (some agents expose one for a linked or remote computer), call it too: it gives the physical machine's name and platform even when no shell on that machine is reachable. Platform values usually map as `win32` = Windows, `darwin` = macOS, `linux` = Linux. If that is the only source for the physical machine, report the OS at the level it gives (e.g. 'Windows') without inventing a version.

### Fallbacks

- Windows: `$env:COMPUTERNAME` (PowerShell) or `echo %COMPUTERNAME%` (cmd); OS via `ver`
- macOS: `scutil --get ComputerName` (the friendly name) or `scutil --get LocalHostName`
- Linux: `cat /etc/hostname`, `uname -n` or `hostnamectl --static`; OS via `uname -sr`

On macOS, `hostname` can return a network-assigned name (e.g. ending `.local` or one handed out by the router). If the result looks like that, also run `scutil --get ComputerName` and report that name, since it is the one the user will recognise.

## Working out what each shell is

A shell will happily return its own hostname whatever it is, so label each one correctly and never put a sandbox or VM name on the Physical line:

- **Cloud sandbox** - hostname looks like a random ID, a generic home or workspace directory (e.g. `/workspace`, `/home/<agent-name>`), not on the user's machine at all.
- **VM on the user's machine** - e.g. an agent's local sandbox VM with the user's folders mounted into it. It is on the computer but is not the computer, so its hostname is the VM's.
- **WSL or a container** on a Windows or Mac host - `uname -r` mentioning `microsoft` or `WSL`, or a `/.dockerenv` file.
- **The physical machine itself** - none of the above signs, and the name matches what the session or a device-information tool says about the user's device.

Linux can be a genuine physical machine, so do not rule a shell out just for being Linux. Judge by the signs above: a hostname that is a random ID, a sandbox-style home directory, or a mismatch with what the session says about the user's device all mean it is not the real machine.

If no shell on the physical machine is reachable and there is no device-information tool, put 'not reachable from this session' on the Physical line and add one sentence giving the user the command (`hostname`) to run themselves if they want to check.

## If a command fails

Try the fallback for that OS. If that also fails, put the error briefly on that line (e.g. 'Linux - hostname failed: <error>') rather than guessing a name.
