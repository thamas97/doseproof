# Layer 1 acceptance register

All rows start **Pending**. Record Met, Partly met, or Blocked only after observing the behavior, saving evidence, and naming a reviewer. Repository setup does not satisfy a plant acceptance check.

| WBS | Observable acceptance | Status | Evidence | Reviewer |
| --- | --- | --- | --- | --- |
| 1.1 | Each story has an observable expected result and a simple acceptance check. | Pending | — | — |
| 1.2 | One changing virtual reading is visible in FUXA without physical hardware. | Pending | — | — |
| 1.3 | Resetting a saved scenario reproduces the same model behaviour. | Pending | — | — |
| 1.4 | A command, its acknowledgement and the resulting equipment behaviour can be distinguished. | Pending | — | — |
| 1.5 | Approved and intentionally unapproved lab changes both leave traceable records. | Pending | — | — |
| 1.6 | The two sources can report different measurements and fail separately in the simulation. | Pending | — | — |
| 1.7 | A beginner can follow the process and distinguish settings, equipment feedback and sensor readings. | Pending | — | — |
| 1.8 | Later connectors have documented source records and repeatable examples to consume. | Pending | — | — |
| 1.9 | The software-only plant is repeatable, demonstrable and ready for elements 2–8 to connect. | Pending | — | — |

## DOS-4 / WBS 1.1.4 — check and evidence definitions

| WBS | Observable documentation acceptance | Status | Evidence | Reviewer |
| --- | --- | --- | --- | --- |
| 1.1.4 | Each story has starting conditions, an action, observable expected result, evidence location and reviewer; six handed-over fault variants are covered. | Pending | [Eleven check definitions and named review record](acceptance-checks.md). | Donald — pending |

**Actual documentation result:** five main checks plus six variants prepared, with step-aligned expectations, proposed tolerances, unique planned evidence filenames and cleanup. All eleven runtime results are **Not run**. No source records, screenshots, real approvals or executed test results are fabricated.

**Named findings:** Katekiso — calculation/timing/tolerances pending; Emmanuel — data/evidence meanings pending; Donald — actors/approval and acceptance pending; Stephen — controller interface pending; Tranos — display interface pending; Thabani — integration/completeness pending. DOS-1/DOS-2/DOS-3 inputs remain pending.

**Board status:** In Progress, documentation prepared and awaiting review capacity. Review held four cards at preparation; moving this card to Review requires fewer than three cards already there. Other cards are not changed.

**Parent WBS 1.1 remains Pending.** DOS-4 acceptance covers agreed definitions, not passed runtime checks; actual execution belongs to later implementation.

## Required scenario observations

- Baseline and approved 15 → 20 mg/s: actual delivery changes before residual tends toward 1.50 mg/L; retain the action change ID.
- Labelled unapproved lab action: actor, command, and acknowledgement remain traceable without an approval.
- Equipment cases separately: no flow, ignored command, stopped pump, reduced delivery.
- Sensor cases separately on A while B continues: missing, delayed, stuck, biased.
- Desk refusal cases: anonymous, self-approved, expired, mismatched, duplicate execution.
- Observer write refusal with unchanged plant state; ticks continue when observer monitoring stops.
- Saved-state startup and repeatable reset, independently performed by another member.

## Release contents still required

Implemented code, verified dependency locks, agreed contracts, scenarios/seeds, saved FUXA export, startup/reset guide, captured runtime records, screenshots, peer review, acceptance results, and an honest backlog. Preserve earlier runs and exclude credentials/session secrets.
