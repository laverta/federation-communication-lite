---
name: federation-communication-lite
description: Lightweight cross-session communication for independent agent sandboxes. Use direct native task messaging first; use a shared Markdown board only when direct communication is unavailable. 中文触发词：联邦通信、桥接、敲门、状态同步、公共板。
---

# Federation Communication Lite

## Purpose

Provide a minimal, platform-neutral communication method for independent agent sessions. This Skill is a message protocol, not a scheduler, manager, auditor, or decision-maker.

## Communication priority

1. Use the platform's native task/thread messaging when available.
2. Use one shared Markdown board only when native messaging is unavailable.
3. Do not start a background service, heartbeat loop, or continuous polling process.

Never duplicate the same message through both native messaging and the shared board unless the coordinator explicitly asks for a fallback record.

## Minimal message

Use this format for read-only status checks and simple routing:

```text
task_id: <short-id>
status: ACK | QUESTION | BLOCKED | RESULT
summary: <one redacted sentence>
next_owner: <role or agent>
```

Use a complete task specification for code changes, releases, safety decisions, external actions, or sensitive work. A minimal message must never bypass approval or ownership rules.

## Shared Markdown board

The coordinator provides the board path for the local environment. Do not hard-code a machine-specific path in this portable Skill.

If the board does not exist, create it with:

```markdown
# Shared Federation Board
```

Append one compact entry:

```markdown
### <timestamp> | from: <agent> | to: <recipient> | type: ACK
task_id: <short-id>
summary: <one redacted sentence>
next_owner: <role or agent>
```

Allowed types: `TASK`, `ACK`, `PROGRESS`, `QUESTION`, `RESULT`, `BLOCKED`.

## Operating rules

- Read the latest relevant board entry before replying to a bridge request.
- Append only; never rewrite or delete history.
- Address a specific agent whenever possible; do not broadcast by default.
- A sent message is not proof of delivery.
- A board entry is not proof that an agent is online, has read the message, or has accepted the task.
- Treat `ACK` as receipt only, not completion.
- Treat `RESULT` as complete only when evidence and acceptance criteria are supplied.
- If a target session is unavailable, report `QUESTION` or `BLOCKED`; do not pretend it was reached.
- After one bounded reminder, stop and report the lack of response. Do not repeatedly knock.
- Completed, paused, archived, or unavailable sessions are not polled.

## Privacy

The board may be visible to every connected sandbox. Never write API keys, passwords, cookies, authorization values, private user data, raw sensitive disclosures, complete payloads, or unnecessary full local paths. Share only a short redacted summary and an artifact reference.

## Scope boundary

Reading or writing the board does not grant permission to modify code, systems, accounts, or external services. The coordinator assigns ownership; the responsible agent decides implementation details within its approved scope.

## First-load acknowledgement

Only when the coordinator explicitly asks you to join the shared board, append:

```markdown
### <timestamp> | from: <your-agent> | to: coordinator | type: ACK
task_id: bridge-onboarding
summary: federation-communication-lite loaded; native messaging remains preferred.
next_owner: coordinator
```
