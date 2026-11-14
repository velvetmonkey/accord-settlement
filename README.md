# accord-settlement

A session guard for AI agents. It stops allowed actions from combining into a leak.

> **Status: placeholder, research programme, starts after Project Tenter.** There is no
> code here yet. This file records the question, the end state and the first build.

## The problem, in three lines

    read the customer list      permitted
    read the pricing model      permitted
    send an ordinary email      permitted

Each effect is individually authorised. Every receipt is honest. Together they are
exfiltration.

## The question

An agent can do many actions that are each allowed. Some combinations of these actions
are a leak. Can a guard see the combination and stop it, without stopping normal work?

[seal](https://github.com/velvetmonkey/seal) approves **one exact effect** and proves it.
That is the correct primitive, and it is narrow on purpose. seal does not track a
session, a total budget or a rule across a sequence of effects. This repository adds
that layer beside seal. It does not change seal.

## The end state

A session guard that runs beside seal, for agents such as Claude Code. The guard:

1. **Tags data at the source.** Each value gets a label when it enters the session:
   `secret`, `untrusted` or `public`.
2. **Carries the tags through the session.** A value made from a tagged value keeps the
   tag.
3. **Blocks flows that cross a label line.** Examples: secret data into an outbound
   channel; untrusted text into a shell command.
4. **Uses seal exact-call approval as the human override.** A person can approve one
   exact call that the guard blocked. The approval covers that call only.
5. **Writes a signed receipt for every block, every pass and every approval.**
6. **Keeps budgets against slow leaks.** A total limit per session stops a leak that
   sends small parts over many calls.
7. **Has a flow invariant that is proved in Lean.** The proof states that no labelled
   value reaches a forbidden sink without a recorded approval.

## The first build

- **Hooks.** Claude Code `PreToolUse` hooks, plus an MCP proxy that sits between the
  agent and its MCP servers.
- **Replay.** Run the guard against a public agent attack set (for example AgentDojo).
- **Score on two things together.** The number of attacks that the guard stops, AND the
  number of normal tasks that the guard leaves alone. A guard that blocks everything
  gets a bad score.

## The showpiece: heist replay

1. Replay one agent run with the guard in log-only mode. Three steps each get a green
   receipt. The three steps together are a red leak.
2. Replay the same run with the guard on. The guard blocks the third step and writes a
   signed receipt that names the flow it stopped.

## Prior art

Other teams work on this problem. This project must do better than them, or it has no
reason to exist.

- **CaMeL** (Google DeepMind, 2025). It separates trusted control flow from untrusted
  data and applies capability rules to tool calls.
- **FIDES** (Microsoft). It applies information-flow control with labels to agent
  planners.

The claimed difference is the combination of three things: seal exact-call approval as
the human override, a signed receipt for each decision, and a flow invariant proved in
Lean. Each of these claims needs proof before we make it in public.

## Settlement semantics, still open

- What happens when four of five dependent commitments execute?
- What happens after a network partition?
- Can a commitment be partially fulfilled?
- Who absorbs settlement failure?
- When rollback is impossible, what compensating action applies?

## The invariants this programme inherits

- **Legibility is not consent.** That something can be inferred does not make every use of it legitimate.
- **Inference is not authority.** An agent may believe I prefer X and still have no authority to do X.
- **Coordination does not require surrender.**
- A valid mandate is **necessary** for action. It is **not sufficient** for legitimacy.
- Use CRDTs to converge the record, not to converge the people.

## Siblings

| Repository | Role |
|---|---|
| [accord-settlement](https://github.com/velvetmonkey/accord-settlement) | The session guard. Stops allowed actions from combining into a leak. |
| [accord](https://github.com/velvetmonkey/accord) | The protocol. Binding deals without disclosure of preferences. |
| [reveal-lab](https://github.com/velvetmonkey/reveal-lab) | The hostile benchmark for LLM mediators. |
| [seal](https://github.com/velvetmonkey/seal) | The finished authority boundary. One exact effect, one receipt. |

A planned essay, "Who can object?", covers agency law for agents and standing for
parties that a deal leaves out.

Licence: [Apache-2.0](LICENSE).
