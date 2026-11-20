<!--
Thanks for contributing. Please read CONTRIBUTING.md before opening this pull request.
Keep each pull request to one logical change.
Security problems should be reported privately (see SECURITY.md), not fixed in a public pull request.
-->

## What this changes

<!-- A short description of the change. -->

## Why

<!-- The problem it solves. Link the issue if there is one, e.g. 'Closes #12'. -->

## Type of change

- [ ] Fix (wrong or confusing result)
- [ ] New behaviour (e.g. better sandbox, VM, WSL or container detection)
- [ ] Change to the output format (breaking)
- [ ] Documentation only

## How I tested it

<!-- There is no automated test suite, so please say what you ran. Replace your real hostname with a placeholder such as OFFICE-PC in anything you paste. -->

- Agent and version:
- Computer's operating system:
- Session environment (local, VM, WSL, container, cloud sandbox):
- Prompt used:

<details>
<summary>Output</summary>

```text

```

</details>

## Checklist

- [ ] The skill is still read-only: no commands that write, delete, install, change settings or need elevated permissions.
- [ ] No network requests, and nothing that sends output off the machine.
- [ ] No scripts or binaries added; the skill is still a single Markdown file.
- [ ] Instructions are agent-neutral, with no tool names specific to one agent.
- [ ] Windows, macOS and Linux still work, or I have explained what I could not test.
- [ ] Examples use placeholder names only, and no real hostnames or personal details appear anywhere in this pull request.
- [ ] `metadata.version` in `SKILL.md` is bumped if the skill's behaviour changed.
- [ ] `README.md` is updated if the change affects what the skill runs, how it is installed or what it outputs.
- [ ] I ran the [Bitdefender AI Skills Checker](https://www.bitdefender.com/en-us/consumer/ai-skills-checker) on my branch and it reported no findings.
