# Claude Code extras

## Handoff skill

[`skills/handoff/`](../skills/handoff/SKILL.md) is for the moment before you run `/clear`. It:

1. saves what the session learnt to memory or to the project's own files,
2. lists what would be lost (uncommitted changes, running tasks, open actions), and
3. writes a short prompt to paste into the next session.

**Install:** copy the `skills/handoff/` folder to `~/.claude/skills/handoff/`. Run it with `/handoff`, or `/handoff <focus for the next session>`. Claude does not start it by itself.

## Subagents

[`agents/`](../agents/) has three subagents, each tied to a model so that cheap work goes to a cheaper model:

| Agent | Model | Use it for |
| --- | --- | --- |
| `coder` | Opus | Writing or changing code, debugging, design. |
| `researcher` | Sonnet | Research, reading docs, summarising, reviewing text. |
| `Explore` | Haiku | File search, lookups, bulk extraction. |

**Install:** copy the files in `agents/` to `~/.claude/agents/`. The "Subagent models" section of the [Claude Code example](../examples/claude-code/CLAUDE.md) tells Claude when to pick each one.

After either install, start a new session so that Claude Code loads the change.
