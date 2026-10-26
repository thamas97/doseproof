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

## DOS-1 / WBS 1.1.1 documentation review

This subtask entry does not close the WBS 1.1 group above or establish any live plant behaviour.

| WBS | Observable acceptance | Status | Evidence | Reviewer |
| --- | --- | --- | --- | --- |
| 1.1.1 / DOS-1 | One agreed treatment-line sketch identifies inlet/flow, chemical injection, dosing pump, tank, outlet and A/B sources, separately labelling settings, delivery and measurements. | Pending | [Scope and review record](scope.md); [PNG](process-sketch.png); [editable SVG](process-sketch.svg) | Donald: pending. |

**Actual documentation result:** The scope and matching SVG/PNG sketch are prepared for review. No implementation, runtime evidence or human approval is implied.  
**Contributor checks:** Tranos (FUXA display feasibility): pending; Stephen (control meanings): pending.  
**Next action:** Record both contributor checks and Donald's review, resolve any comments, then record acceptance. DOS-2 through DOS-4 remain separate work.

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
