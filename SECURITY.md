# Security policy

check-hostname is a single Markdown skill file with no scripts, binaries or dependencies. It instructs an AI agent to run a small set of read-only commands (`hostname` and OS version lookups) and report the result. This policy explains what counts as a security problem here and how to report one.

## Supported versions

Only the latest version on the `main` branch is supported. Fixes are made there and are not back-ported. The current version is shown in `metadata.version` in `skills/check-hostname/SKILL.md`.

| Version | Supported |
|---|---|
| Latest on `main` | Yes |
| Anything older | No |

## What to report

Please report anything in this repository that could cause an agent to act unsafely, for example:

- instructions that could lead an agent to run commands that write, delete, install or change settings
- instructions that could cause output (hostname, OS details or anything else) to be sent off the machine
- wording that could be exploited for prompt injection, or that weakens an agent's own safety checks
- hidden or obfuscated content in any file
- a real hostname, username, path or other personal data committed by mistake

## What is out of scope

- Vulnerabilities in the agent running the skill (Claude, Codex, Gemini CLI, Cursor and so on) - report those to the agent's vendor.
- Vulnerabilities in the operating system commands the skill calls.
- The fact that the skill's output contains your hostname and OS version. That is its purpose, and it is described in the README's privacy note.
- Modified copies of the skill distributed elsewhere. Only the files in this repository are covered.

## How to report

**Please do not open a public issue for a security problem.**

Report privately through GitHub:

1. Go to the repository's [Security tab](https://github.com/joermwatts/check-hostname-skill/security).
2. Click **Report a vulnerability**.
3. Describe the problem, including the file and lines involved, the agent and OS you used, and what the agent did or could be made to do.

Redact your own hostname and any other personal details from anything you paste.

## What to expect

This project is maintained by one person in their spare time, so the timings below are aims and not guarantees.

- **Acknowledgement:** within 7 days.
- **Assessment:** an initial view on whether it is accepted, and how serious it is, within 14 days.
- **Fix:** as the skill is one Markdown file, accepted issues are normally fixed on `main` shortly after assessment.
- **Disclosure:** once a fix is published, the report is disclosed through a GitHub security advisory, with credit to you unless you ask not to be named.

If a report is declined, you will be told why.

## Checking the skill yourself

You can read everything the skill does in `skills/check-hostname/SKILL.md`. You can also scan the repository with a third-party tool such as the [Bitdefender AI Skills Checker](https://www.bitdefender.com/en-us/consumer/ai-skills-checker) before installing it.
