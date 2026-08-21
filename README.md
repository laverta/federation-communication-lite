# Federation Communication Lite

![skill](https://img.shields.io/badge/skill-v1-1c6795) ![status](https://img.shields.io/badge/status-ACTIVE-176b3a) ![scope](https://img.shields.io/badge/scope-portable%20%C2%B7%20redacted-59636e)

> ### Communicate less. Coordinate better.

A small, platform-neutral protocol for independent agent sessions. Native thread messages first. A shared Markdown board only when a direct connection does not exist.

**`Native messaging` > `Shared Markdown fallback` > `On-demand bridge`**

## Communication priority

| # | Route | When it applies |
|---|---|---|
| 1 | **Native task message** â€” send / read / wait / acknowledge | Always, when the platform supports it |
| 2 | **Shared Markdown board** â€” redacted, append-only, manual fallback | Only when direct messaging is absent |
| 3 | **Legacy / on-demand bridge** â€” automated bus and service bridge | Stays **off** unless explicitly approved |

> ACK confirms receipt, not completion.

## Built for bounded work

Communication is a delivery layer, not the management system.

**01 â€” Single owner.**
Every construction task has one accountable owner, a bounded write scope, and a verification route.

**02 â€” Small context.**
Send one redacted summary and artifact references instead of copying entire conversations between sessions.

**03 â€” No idle loops.**
One bounded reminder is enough. Completed, paused, archived, and unavailable sessions are not polled.

## Minimal message contract

![ACK](https://img.shields.io/badge/ACK-received%2C%20not%20finished-176b3a) ![QUESTION](https://img.shields.io/badge/QUESTION-ask%20before%20acting-9a6700) ![BLOCKED](https://img.shields.io/badge/BLOCKED-report%2C%20never%20invent-cf222e) ![RESULT](https://img.shields.io/badge/RESULT-evidence%20required-1c6795)

```text
task_id: docs-014
status: ACK
summary: Received the communication policy update.
next_owner: coordinator
```

- ACK means received, not finished.
- RESULT needs evidence and acceptance criteria.
- Unavailable sessions are reported, never invented.
- Do not duplicate the same message through both native messaging and the shared board.

## Install

1. Copy `SKILL.md` into your assistant's skills directory.
2. That's it. The protocol is deliberately short â€” read it once.
3. Optional: open `sample.html` for a visual overview of the design.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The skill itself â€” protocol, operating rules, privacy |
| `HANDOFF.md` | Integration notes for agent harnesses |
| `logo.html` | Canvas logo generator â€” open in a browser, download as PNG |
| `sample.html` | Visual one-page overview of the whole design |

## Privacy

The shared board may be visible to every connected sandbox. Never write API keys, passwords, cookies, authorization values, private user data, raw sensitive disclosures, or unnecessary full local paths. Share only a short redacted summary and an artifact reference. A sent message is not proof of delivery.