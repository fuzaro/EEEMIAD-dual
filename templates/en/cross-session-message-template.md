# ✉️ CROSS-SESSION MESSAGE · [subject]

*Closed packet sent from a deciding session to a session on the terrain (see [docs/en/03](../../docs/en/03-cross-session-communication.md))*

> [!IMPORTANT]
> Copy from the "First line" line on and send it **all at once**. Include no secrets. For writes, the operator must also authorize in the acting session.

```text
First line: [READ-ONLY | AUTHORIZED WRITE] · [what it is, in one sentence] · [system/arc]

Context: [who asks, why, the operator's decision and date if it is a write]

DO NOT: [e.g. do not run job run · write nothing · print no tokens or passwords]

Steps (reply to each with the command run and the observed output):
1. [command or check]
2. [command or check]
3. [...]

If something blocks (missing credential, 403, divergence from expected): stop and reply with the output; do not look for the credential on your own.

Reply format: one block per step · command · output (summarized if long) · secrets omitted.
Reply to: [deciding session's name]
```

## Correction (only if needed, once)

```text
First line: Correction of item [N] of the previous message · [subject]
What is right: [correct, complete instruction]
Restriction restated: [e.g. nothing runs without the token and the operator's authorization there]
```

## Record (deciding session, in the arc's charter)

| Message | Action | Key 1 | Key 2 | Reply received | Re-run from here | R15 level |
|---|---|---|---|---|---|---|
| [subject] | read / write | [decision, date] | [authorization in the acting session] | [yes, date / no] | [command → result] | [2 / 3b / 4] |

After recording it in the charter, add a line to the project's evidence journal (`eee/log.md`) when the round changed the state of an environment.
