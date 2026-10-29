# ai-tooling

![works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Codex%20%7C%20Claude%20%7C%20ChatGPT-blue)
![licence](https://img.shields.io/badge/license-MIT-green)

Instructions, a starter kit, a skill and subagents that make AI assistants answer in short, structured, honest replies and help you run several AI coding sessions on one project. Copy the parts you want.

## What is inside

| Part | What it does |
| --- | --- |
| [Starter kit](starter-kit/README.md) | Your AI tool reads these files, asks you a few questions and sets everything up for you. |
| [Examples](docs/examples.md) | My own instructions for Claude Code, Codex, Claude and ChatGPT, with personal details replaced by placeholders. |
| [Skills and agents](docs/skills-and-agents.md) | A handoff skill for clearing a session safely, and three agents tied to model tiers. For Claude Code, Codex and other agents. |

## Getting started

### Claude Code

1. Download `starter-kit/1-response-style.md` from this repository. In a Claude Code session, give it the file and say: "Read this file and follow its instructions."
2. Claude Code asks you a few questions, shows you the instructions it will write to `~/.claude/CLAUDE.md`, and writes them when you agree.
3. Optional: install the [handoff skill and agents](docs/skills-and-agents.md).
4. Start a new session so that Claude Code loads the changes.

### Codex

1. Download `starter-kit/1-response-style.md` from this repository. In a Codex chat, give it the file and say: "Read this file and follow its instructions."
2. Codex asks you a few questions, shows you the instructions it will write to `~/.codex/AGENTS.md`, and writes them when you agree.
3. Optional: install the [handoff skill and agents](docs/skills-and-agents.md).
4. Start a new chat so that Codex loads the changes.

### Claude and ChatGPT

Give the same file to the app and follow its steps. It tells you which settings to paste each part into. See [Examples](docs/examples.md) for a finished version.

### Several sessions on one project

When you run more than one AI session on a project, give your tool [`starter-kit/2-multi-session-framework.md`](starter-kit/2-multi-session-framework.md) in the same way, and start with phase 1. To let the sessions hand work to each other with nobody typing, see [session-queue](https://github.com/niklasrichardson/session-queue).

## Licence

[MIT](LICENSE).
