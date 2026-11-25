# Skills and agents

A handoff skill and three agents. They work in Claude Code and Codex. Other agents can use the same text as their own instructions.

After any install, start a new session so that the tool loads the change.

## Handoff skill

[`skills/handoff/`](../skills/handoff/SKILL.md) is for the moment before you clear, compact, restart or hand over a session. It:

1. saves what the session learnt to the tool's memory or to the project's own files,
2. lists what would be lost (uncommitted changes, running tasks, open actions), and
3. writes a short prompt to paste into the next session.

The skill follows the open [Agent Skills](https://agentskills.io/specification) standard. The folder has two files:

| File | Used by |
| --- | --- |
| `SKILL.md` | All tools. |
| `agents/openai.yaml` | Codex only. Other tools ignore it. |

### Install the skill

Copy the whole `skills/handoff/` folder:

| Tool | Copy the folder to | Start it with |
| --- | --- | --- |
| Claude Code | `~/.claude/skills/handoff/` | `/handoff` |
| Codex | `~/.agents/skills/handoff/` | `$handoff`, or pick it from `/skills` |
| Other agents | The tool's skills folder, if it supports Agent Skills. If not, paste the text below the `---` header of `SKILL.md` as a prompt. | |

To focus the handoff on something, add it to the same message, for example `/handoff finish the login tests` in Claude Code or `$handoff finish the login tests` in Codex.

On Windows, the Codex folder is probably `C:\Users\<you>\.agents\skills\handoff\`. The Codex docs do not show a Windows path, so this is not verified.

### Manual or automatic start

- **Claude Code** can start the skill on its own when you say you are about to clear. To make it manual-only, add `disable-model-invocation: true` to the `---` header of your installed copy of `SKILL.md`.
- **Codex** starts it only when you call it, because `agents/openai.yaml` sets `policy.allow_implicit_invocation: false`. To let Codex start it on its own, delete that file from your installed copy.

## Agents

There are six agents. Each has a tier, so that cheap work goes to a cheaper model or a lower reasoning effort:

| Agent | Tier | Claude Code model | Codex reasoning effort | Use it for |
| --- | --- | --- | --- | --- |
| `senior-reviewer` | Best | Fable | `xhigh` | Read-only code review of a diff or commit range, when you ask for it. |
| `plan-reviewer` | Best | Fable | `xhigh` | Read-only review of a plan or an escalation before anything is built, when you ask for it. |
| `coder` | Strongest | Opus | `xhigh` | Writing or changing code, debugging, design. |
| `ui-coder` | Strongest | Opus | None (Claude Code only) | Coding that must preview a UI in the browser pane or publish an Artifact. |
| `researcher` | Middle | Sonnet | `medium` | Research, reading docs, summarising, reviewing text. |
| `explore` | Fast | Haiku | `low` | Read-only file search, lookups, bulk extraction. |

The files are in three folders:

| Folder | What it holds |
| --- | --- |
| [`agents/roles/`](../agents/roles/) | The shared source for each agent: name, description, tier, read-only, and the instructions. |
| [`agents/claude-code/`](../agents/claude-code/) | Claude Code agent files, generated from the roles. |
| [`agents/codex/`](../agents/codex/) | Codex agent files, generated from the roles. |

### Install the agents

| Tool | Copy | To |
| --- | --- | --- |
| Claude Code | The files in `agents/claude-code/` | `~/.claude/agents/` |
| Codex | The files in `agents/codex/` | `~/.codex/agents/` for all your projects, or `.codex/agents/` in one project |
| Other agents | The text below the `---` header of each file in `agents/roles/` | The tool's own agent or system instructions |

**Claude Code notes:**

- The explore agent is named `Explore`, with a capital E. It replaces Claude Code's built-in Explore agent. Keep that name.
- A `tools:` line limits `Explore` and the two reviewers to tools that read. The reviewers also have no web tools.
- `coder` cannot use the built-in browser pane or the Artifact tools of the Claude desktop app. In one measurement, their schemas added about 21K tokens to every coder request, and fewer than 3% of coder tasks used them. `ui-coder` is the same agent with those tools kept. Where these tools do not exist, the limit has no effect.
- The two reviewers use Fable. If your account does not have Fable, change `tier: best` to `tier: strongest` in their role files and rebuild.
- The "Subagent models" section of the [Claude Code example](../examples/claude-code/CLAUDE.md) tells Claude when to pick `coder`, `researcher` and `Explore`. Claude picks `ui-coder` and the reviewers from their descriptions.

**Codex notes:**

- Replace the model `gpt-6.1-sol` with a model your account has.
- A Codex agent file cannot set `sandbox_mode`. The explore agent and the two reviewers stay read-only only because their instructions tell them not to change files. Codex does not enforce this.
- There is no Codex `ui-coder`, because its tools exist only in the Claude desktop app.

### Change an agent

Do not edit the files in `agents/claude-code/` or `agents/codex/` by hand. Edit the role file in `agents/roles/`, then run:

```text
python scripts/build_agents.py
```

The script uses only the Python standard library. A role file has these fields in its `---` header:

| Field | Value |
| --- | --- |
| `name` | The agent name. Must match the file name. |
| `description` | When to use the agent. |
| `tier` | `best`, `strongest`, `middle` or `fast`. |
| `read_only` | `yes` or `no`. `yes` adds the read-only `tools:` line for Claude Code. |
| `claude_code_name` | Optional. A different name for the Claude Code agent. |
| `claude_code_description` | Optional. A different description for the Claude Code agent. |
| `claude_code_tools` | Optional, read-only roles only. A comma-separated list that replaces the default read-only tools. |
| `claude_code_disallowed_tools` | Optional. A comma-separated list of tools that the Claude Code agent cannot use. |
| `claude_code_only` | Optional. `yes` writes no Codex agent file. |

The text below the header is the agent's instructions. Keep it free of tool-specific names, because every tool uses the same text.
