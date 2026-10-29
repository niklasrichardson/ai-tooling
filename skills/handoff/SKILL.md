---
name: handoff
description: Prepare to clear the session. Save what was learnt to memory or project files, then give a short handoff prompt for the next session. Use when the user says they are about to clear, compact, restart or hand over.
---

The user is about to clear, compact, restart or hand over the session. Everything only in this conversation will be lost. Do these steps in order.

## 1. Save what must survive

Go through the conversation and save each item to the right place. Do not save anything twice.

- **Memory** (the tool's memory, if it has one): facts about the user, their preferences, feedback on how to work, and project context that the repo does not record. Update an existing memory entry instead of creating a duplicate. Delete memories this session proved wrong.
- **Project files**: decisions, findings and plans that affect future work go into the project's own docs, issue, ADR, TODO or agent instruction file (such as `AGENTS.md` or `CLAUDE.md`), whichever the project already uses. Follow the project's existing conventions.
- **Work in progress**: if the next session needs scratch notes, intermediate results or a plan, write them to a file in the project (not a temporary or scratch folder, which is not kept).
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
- Leave out history, dead ends and anything already saved to memory, if the tool loads memory into new sessions by itself.

If the user named a focus for the next session in their request, focus the handoff on it.

## 4. Reply

Reply in this order:

1. What you saved and where (one line per file, as clickable paths where the tool supports them).
2. Anything from step 2 the user must act on before clearing. Make these prominent.
3. The handoff prompt in a fenced `text` code block so it is easy to copy.
