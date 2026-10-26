# Scope and architecture

The Layer 1 increment is one local, software-only plant. Collection, normalization, validation, and assurance verdicts belong to later layers.

## Proposed module boundaries

| Module | Responsibility |
| --- | --- |
| `model.py` | Flow, contact-tank residual, fixed simulation tick, pause/reset |
| `controller.py` | Requested/accepted feed, acknowledgement, actual delivery, equipment faults, Modbus snapshots |
| `sensors.py` | Sample modeled residual with independent source IDs and random generators |
| `desk.py` | Named-user login, requests, different-user approval, execution, labelled lab scenario |
| `events.py` | Source envelope and append-only JSONL writer |
| `app.py` | One engine loop, lifecycle, desk queue, service integration |

These are planned files, not implemented modules. One loop owns mutable plant state. Desk commands enter through a queue; web request handlers must not become extra plant-state writers. Disable the web debug reloader so startup cannot create duplicate engines.

## Teaching model

See [DOS-2 teaching settings](teaching-settings.md) for the baseline glossary, explicit four-step delay example and lifecycle meanings. This specification is prepared for review; named agreement and live implementation remain pending. The proposal below is retained unchanged.

At positive flow, `target = max(0, actual_feed_mg_s / flow_l_s - demand_mg_l)`. Delay the response four ticks, then move residual one fifth of the difference toward the delayed target each tick. A one-second tick, 10 L/s, 15 mg/s, demand 0.50 mg/L, and initial residual 1.00 mg/L form the baseline. A valid approved step to 20 mg/s tends toward 1.50 mg/L after the delay.

No flow means zero actual delivery without division. A stopped pump delivers zero; reduced delivery uses factor 0.5; an ignored command retains the prior accepted feed. Keep requested feed, accepted feed, actual delivery, acknowledgement, and sensor values separate.

Pause freezes simulation steps. Reset clears command/delay queues and faults, restores initial state and seeds, creates a fresh run ID, and preserves previous records. Compare repeatability by simulation step and values; fresh IDs and UTC timestamps are expected.

## Trust boundaries

The authenticated desk executes normal commands only after a different named user approves the matching request and value/range before expiry. Refuse anonymous requests, self-approval, expired/mismatched approval, and duplicate execution on the server. Store password hashes and keep session secrets outside Git.

FUXA is an observer. Server-side observer writes must be denied and verified to leave state unchanged; removing screen controls alone does not enforce this boundary. The lab-only unapproved path must be explicitly labelled and leave actor and command records.

Sensors A/B share the same modeled residual and laptop. Distinct seeds and source IDs permit separate faults but do not provide physical independence.
