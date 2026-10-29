---
name: handoff
description: Prepare to clear the session. Save what was learnt to memory or project files, then give a short handoff prompt for the next session. Use when the user says they are about to clear, compact, restart or hand over.
argument-hint: "[focus for the next session]"
disable-model-invocation: true
---

The user is about to run `/clear`. Everything only in this conversation will be lost. Do these steps in order.

## 1. Save what must survive

Go through the conversation and save each item to the right place. Do not save anything twice.

- **Memory** (per the memory instructions in the system prompt): facts about the user, their preferences, feedback on how to work, and project context that the repo does not record. Update an existing memory file instead of creating a duplicate. Delete memories this session proved wrong.
- **Project files**: decisions, findings and plans that affect future work go into the project's own docs, issue, ADR, TODO or `CLAUDE.md`, whichever the project already uses. Follow the project's existing conventions.
- **Work in progress**: if the next session needs scratch notes, intermediate results or a plan, write them to a file in the project (not the scratchpad, which is not kept).
- **Do not save** what the code, git history or existing docs already record, or anything only relevant to this conversation.

Do not commit, push or delete anything unless the user has already asked for that in this session.

## 2. Check for things that will be lost

List any of these that apply:

- uncommitted changes (run `git status` if in a repo)
- background tasks, subagents or dev servers still running
- unresolved actions the user has not done yet
- open questions or unverified assumptions

## 3. Write the handoff prompt

Write a prompt the user can paste into the new session. Keep it short, because the next session re-reads its whole context on every request:

- Target under 250 words.
- Point to files by path instead of copying their content.
- Include: the goal, current state, the next concrete step, and decisions already made (so they are not re-litigated).
- Include warnings and blockers.
- Leave out history, dead ends and anything already saved to memory (memory loads automatically).

If the user gave an argument, focus the handoff on it: $ARGUMENTS

## 4. Reply

Reply in this order:

1. What you saved and where (one line per file, as clickable paths).
2. Anything from step 2 the user must act on before clearing. Make these prominent.
3. The handoff prompt in a fenced `text` code block so it is easy to copy.
