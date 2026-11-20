# Contributing to check-hostname

Thanks for taking the time to help. This is a small project - one agent skill in one Markdown file - so contributing is meant to be quick and low on ceremony.

## Ways to contribute

- **Report a problem** - the skill gave a wrong or confusing answer in your agent.
- **Report compatibility** - tell us it works (or doesn't) in an agent or OS not yet covered.
- **Improve the skill** - better detection of sandboxes, VMs, WSL or containers, clearer wording, extra fallbacks.
- **Improve the docs** - corrections to the README, especially the per-agent install paths.

## Reporting an issue

Search [existing issues](https://github.com/joermwatts/check-hostname-skill/issues) first, then open a new one with:

- the agent and version you used (e.g. Claude Code, Codex CLI, Gemini CLI, Cursor)
- the operating system of your computer, and whether the session was local, in a VM, WSL, a container or a cloud sandbox
- what the skill reported and what you expected instead
- the prompt you used, if it affects triggering

**Redact before you post.** The skill's output contains your computer's hostname and OS version. Replace real names with placeholders such as `OFFICE-PC` before pasting output into a public issue.

## Proposing a change

For anything beyond a typo or small fix, open an issue first so the approach can be agreed before you spend time on it.

1. Fork the repository and create a branch from `main` with a short descriptive name (e.g. `fix-wsl-detection`).
2. Make your change. Keep each pull request to one logical change.
3. Test it (see below).
4. If you changed the skill's behaviour, bump `metadata.version` in the front matter of `skills/check-hostname/SKILL.md` using [semantic versioning](https://semver.org/): patch for fixes, minor for new behaviour, major for changes to the output format.
5. Update `README.md` if the change affects what the skill runs, how it is installed or what it outputs.
6. Open a pull request describing what changed, why, and how you tested it.

## Rules for the skill

These keep the skill safe to install and useful in every agent. Pull requests that break them will not be merged.

- **Read-only.** The skill may only run commands that read the machine's name and OS details. Nothing that writes files, changes settings, installs software or needs elevated permissions.
- **No network.** No commands that make network requests, and no instructions to send output anywhere.
- **No scripts or binaries.** The skill stays as a single Markdown file so anyone can read everything it does. If you think a script is needed, open an issue to discuss it first.
- **Agent-neutral.** No tool names or features specific to one agent in the instructions. Describe capabilities generically (e.g. 'a device-information tool') and keep to the open [Agent Skills](https://agentskills.io) format.
- **Cross-platform.** Changes must not break Windows, macOS or Linux. If you add a command for one OS, say what the equivalent is for the others, or why there isn't one.
- **No personal data.** Examples use placeholder names only - no real hostnames, usernames, paths or company names.
- **Stable output.** The three-part output (Physical, Available, Chat using) is the skill's contract. Changing its shape is a breaking change and needs an issue first.
- **Explain the why.** Instructions should give the reasoning behind a step rather than a bare rule, so agents can handle cases the skill did not anticipate.

## Testing your change

There is no automated test suite. Before opening a pull request:

1. Install your modified skill in at least one agent and ask it something like 'which machine is this chat running on?'.
2. Check that all three parts of the output appear and that a sandbox or VM name is not reported as the physical computer.
3. Run the skill through the [Bitdefender AI Skills Checker](https://www.bitdefender.com/en-us/consumer/ai-skills-checker) (link to your fork or branch) and confirm it reports no findings.
4. In the pull request, state which agent, OS and environment you tested in. Testing in more than one is welcome but not required.

## Style

- Plain Markdown, no HTML.
- British English spelling.
- Short sentences and imperative instructions ('Run `hostname`', not 'The agent should run `hostname`').
- Use a plain hyphen with spaces ( - ) rather than an em dash.

## Security concerns

If you spot anything in the skill that could cause an agent to do something unsafe, please open an issue describing it. As the skill is a public, read-only Markdown file, there is nothing sensitive to disclose privately.

## Conduct

Be respectful and constructive. Assume good intent, keep discussion about the work, and remember that people using this project have very different levels of technical experience.

## Licence

By contributing, you agree that your contributions are licensed under the [MIT Licence](LICENSE) that covers this project.
