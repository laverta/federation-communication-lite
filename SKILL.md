---
name: federation-communication-lite
description: Lightweight cross-session communication for independent agent sandboxes. Use direct native task messaging first; use a shared Markdown board only when direct communication is unavailable. Every message and artifact carries a provenance tag identifying the model that produced it. 中文触发词：联邦通信、桥接、敲门、状态同步、公共板。
---

# Federation Communication Lite

## Purpose

Provide a minimal, platform-neutral communication method for independent agent sessions. This Skill is a message protocol, not a scheduler, manager, auditor, or decision-maker.

## Communication priority

1. Use the platform's native task/thread messaging when available.
2. Use one shared Markdown board only when native messaging is unavailable.
3. Do not start a background service, heartbeat loop, or continuous polling process.

Never duplicate the same message through both native messaging and the shared board unless the coordinator explicitly asks for a fallback record.

## Model provenance (who wrote this)

A session and its model are two different things: the conversation can switch models mid-stream, and the new model cannot detect the switch — it will treat everything in context as its own words. Therefore identity is declared, not inferred.

- Every substantive output (analysis, plan, artifact, commit) ends with a provenance tag: `[by: <model-name>]`.
- In message and board formats, the `by:` field is **mandatory**. A message or board entry without `by:` is invalid and may be returned to the sender.
- When a session reports work done before a model switch, it tags the current model and marks inherited work as `provenance: inherited`.
- The coordinator (the human) holds final authority over attribution disputes.

### Verification (three layers)

y: is a claim, not a fact. A model can claim any identity - a Flash can sign as a Pro. Verify in three layers:

1. **Claim layer**: the y: tag itself - what the model declares.
2. **Execution layer**: claimed identity should be consistent with observed behavior (a model claiming Pro should show Pro-level reasoning depth). Soft check, human-judged.
3. **Coordinator layer**: only the coordinator knows which model is actually running; the coordinator may spot-check key entries against reality.

Never treat y: as verified truth on its own.

## Artifacts

Provenance tags belong on the work itself, not just the envelope. Any content artifact (edited file, written document, generated script) carries [by: <model-name>] inside it, and its commit message includes the model name. An artifact without a tag is treated as unverified.

## Minimal message

Use this format for read-only status checks and simple routing:

```text
task_id: <short-id>
status: ACK | QUESTION | BLOCKED | RESULT
summary: <one redacted sentence>
next_owner: <role or agent>
by: <model-name>
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
### <timestamp> | from: <agent> | by: <model-name> | to: <recipient> | type: ACK
task_id: <short-id>
summary: <one redacted sentence>
next_owner: <role or agent>
```

Allowed types: `TASK`, `ACK`, `PROGRESS`, `QUESTION`, `RESULT`, `BLOCKED`.

## Operating rules

- Read the latest relevant board entry before replying to a bridge request.
- Append only; never rewrite or delete history.
- Address a specific agent whenever possible; do not broadcast by default.
- Declare your model identity in outputs; never claim authorship of inherited context you cannot verify.
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
### <timestamp> | from: <your-agent> | by: <model-name> | to: coordinator | type: ACK
task_id: bridge-onboarding
summary: federation-communication-lite loaded; native messaging remains preferred.
next_owner: coordinator
```
