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

## DOS-2 / WBS 1.1.2 — teaching-settings documentation

| WBS | Observable acceptance | Status | Evidence | Reviewer |
| --- | --- | --- | --- | --- |
| 1.1.2 | Units, initial state, tick/delay and pause/reset meanings are agreed and explicitly labelled teaching assumptions. | Pending | [Teaching settings and calculated examples](teaching-settings.md); named reviews pending. | Donald — pending |

**Actual documentation result:** existing baseline values retained; glossary, baseline/approved-step calculations, four unchanged updates followed by a step-5 response, lifecycle meanings and source-data limitations prepared for review. These are calculated expectations, not captured runtime evidence.

**Named checks:** Katekiso — calculation/timing pending; Emmanuel — units/identity/timestamps/missing-data pending; Donald — DOS-2 acceptance pending. DOS-1's scope review is a pending input; this independent change does not accept or import its unmerged deliverables.

**Parent WBS 1.1 remains Pending.** DOS-3/DOS-4 stories and checks, runtime implementation and live validation remain separate work. Documentation verification does not substitute for human agreement.

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
