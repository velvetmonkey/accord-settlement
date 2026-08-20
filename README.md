# accord-settlement

Stateful commitments above the effect boundary.

> **Status: placeholder.** There is no code here yet. This repository exists so the
> idea is not forgotten and the name is reserved.

## The problem, in three lines

    read the customer list      permitted
    read the pricing model      permitted
    send an ordinary email      permitted

Each effect is individually authorised. Every receipt is honest. Together they are
exfiltration.

[seal](https://github.com/velvetmonkey/seal) authorises **one exact effect** and proves
it. That is the right primitive and it is deliberately narrow. Nothing in it tracks a
session, a cumulative budget, or a constraint across a sequence of effects. So a caller
holding a set of individually valid mandates can compose an outcome that no single
mandate would have permitted.

This is the estate's dominant defect family in a new costume: every check is green and
measures its own subject correctly, while the thing that matters is measured by nobody.

## What this would need

    SessionState
    CumulativeBudget
    ActionHistory
    CompositionConstraint
    CommitmentGraph
    Dependency
    AtomicGroup
    CompensationAction

## Settlement semantics, all unanswered

- What happens when four of five dependent commitments execute?
- What happens after a network partition?
- Can a commitment be partially fulfilled?
- Who absorbs settlement failure?
- When rollback is impossible, what compensating action applies?

## Deliberate viscosity

Faster settlement is not the goal. Some domains need friction on purpose:

    CoolingOffPeriod  HumanOnlyDecision  NonTransferableRight
    NonDelegableAuthority  CircuitBreaker  RateLimit  MandatoryReview

The target is not commitments per second. It is more legitimate optionality, cheaper
refusal, and safer settlement.

## The invariants this programme inherits

These hold across every repository in the set. They are not decoration.

- **Legibility is not consent.** That something can be inferred does not make every use of it legitimate.
- **Inference is not authority.** An agent may believe I prefer X and still have no authority to do X.
- **Coordination does not require surrender.**
- A valid mandate is **necessary** for action. It is **not sufficient** for legitimacy.
- Use CRDTs to converge the record, not to converge the people.

## The type chain

No stage may impersonate the next. Each transition owes a two-sided test: red on the
illegal construction, green on the legal one.

    Fact
      -> PreferenceEvidence
      -> PreferenceHypothesis
      -> Endorsement
      -> Mandate
      -> Commitment
      -> Effect
      -> OutcomeReceipt

## Siblings

| Repository | Role |
|---|---|
| [accord](https://github.com/velvetmonkey/accord) | The protocol. Opaque acceptance, minimal disclosure. |
| [accord-settlement](https://github.com/velvetmonkey/accord-settlement) | Stateful commitments and composition above the effect boundary. |
| [accord-contest](https://github.com/velvetmonkey/accord-contest) | Standing, challenge, remedy, precedent. |
| [reveal-lab](https://github.com/velvetmonkey/reveal-lab) | The hostile benchmark. Lets the central claim die cheaply. |
| [preference-kernel](https://github.com/velvetmonkey/preference-kernel) | Optional private cognition, behind the sovereign boundary. |
| [agency-boundary](https://github.com/velvetmonkey/agency-boundary) | Roles, delegation and standing for persistent agents. |
| [seal](https://github.com/velvetmonkey/seal) | The finished authority boundary. One exact effect, one receipt. |

Licence: not yet chosen.
