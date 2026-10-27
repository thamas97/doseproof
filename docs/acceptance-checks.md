# DOS-4: Acceptance checks and evidence definitions

**WBS:** 1.1.4 | **Owner:** Thabani | **Reviewer:** Donald  
**Documentation:** Prepared; agreement pending; awaiting review capacity.  
**Jira/task-board status:** In Progress. **Runtime results:** all Not run.

> These eleven checks define intended behaviour for a software-only teaching simulation. No runtime check has been executed or passed by creating this document. Values and tolerances are educational proposals for team review, not real treatment operating limits or proof of drinking-water safety.

## Inputs, actors and scope

[DOS-1 / PR #1](https://github.com/thamas97/doseproof/pull/1), [DOS-2 / PR #2](https://github.com/thamas97/doseproof/pull/2) and [DOS-3 / PR #3](https://github.com/thamas97/doseproof/pull/3) remain pending inputs. The checked source snapshots are [DOS-2 settings at dbfb97d](https://github.com/thamas97/doseproof/blob/dbfb97d566aceaf444b25d01880a2b674bd1348b/docs/teaching-settings.md) and [DOS-3 stories at 7d92746](https://github.com/thamas97/doseproof/blob/7d9274666dcf2a8a93b675af3e9a27eedc41a5f1/docs/story-checks.md). This independent branch starts from main and does not import or accept their unmerged files. Reconcile relevant review corrections before accepting dependent check definitions.

**Illustrative scenario actors:** Emmanuel requests and observes; Donald is the different-user approver; Stephen operates labelled lab/fault controls. **Actual task ownership:** Thabani coordinates the specification; Donald reviews acceptance. An in-story approval is not reviewer sign-off or proof an account/action exists. Both sensors share the modeled water and laptop, so separate IDs and seeds do not establish physical independence.

Use the unchanged [architecture](architecture.md), [baseline scenario](../scenarios/baseline.json), [register map](register-map.md), [event contract](event-schema.md) and [runbook](reference/runbook.txt). This task adds no runtime code, public API, event schema, register address or saved scenario setting. Exact UI commands and event-type names depend on later implementation; do not invent working endpoints or launch instructions.

## Common preparation and result rules

1. When the plant exists, start every check from a fresh reset/run: restore baseline values, source seeds, command/model/sample queues and normal fault modes. Preserve earlier evidence and record the new run ID, scenario, settings/seeds and actual installed versions. Keep faults disabled except the single focal fault in that check.
2. Establish the check's relative step 0 and retain actual source/model records. Compare matching simulation steps/snapshots; UTC timestamps remain real source times. Sensor A/B read modeled water, not a feed setpoint or FUXA display. If model truth is not observable in retained implemented model output/trace, record the missing prerequisite rather than substitute a target setting or invent a new register.
3. For a settled-approved 20 mg/s start, Emmanuel creates a fresh request, Donald approves the matching request and the controller executes it normally. Advance 64 simulation updates after execution, then verify model residual is within ±0.000001 mg/L of 1.50 mg/L and delivery is 20 mg/s. The four-step delay leaves 60 response updates: theoretical remaining error is `0.50 * 0.8^60`, approximately 0.000000766248 mg/L. If the actual precondition is not met, do not apply the fault or report a pass.
4. Record one focal action after its declared setup. Do not reuse a prior run's approval or assume another check already ran. Retain actor/command/acknowledgement/feedback distinctions and matching approvals where applicable. Sensor `change_id` normally remains null; time proximity alone does not establish command correlation.
5. All definitions below start **Not run**. Later record actual observations as Pass, Fail or Blocked with a named reviewer; missing evidence or an unmet prerequisite cannot become a Pass. A result is Blocked if it cannot be evaluated and Fail if observed behaviour contradicts a criterion. Documentation acceptance is separate from runtime results.

## Proposed comparison criteria for review

| Quantity or condition | Proposed criterion | Meaning and limit |
| --- | --- | --- |
| Model arithmetic | ±0.000001 mg/L | Compare unrounded model values at matching simulation steps using the delayed recurrence. |
| Normal raw sensor reading | ±0.01 mg/L from corresponding model truth | Applies at the original sample step; not a guarantee of physical accuracy. |
| Quantized physical-value display | At most one 0.01-unit register increment from the corresponding source snapshot | Applies only to scaled flow/feed/residual quantities. Decode once. Quality codes, age and counters retain their own units/codes. |
| Five-second delayed A | 5–6 real seconds from original sample time to emission | Real-time one-second ticks, no pause; proposed allowance is at most one additional tick, never early emission. |
| Missing A | No new A samples for ten steps; unavailable by the window's end | Plant/B continue, retained A age increases and no fresh zero or sample is fabricated. This is a proposed acceptance criterion, not an existing implemented timeout. |

For the approved step: at positive flow, target is `max(0, actual delivery / flow - demand)`. At 10 L/s, 15 mg/s delivery and 0.50 mg/L demand it is 1.00 mg/L; 20 mg/s gives 1.50 mg/L. A change before relative step 1 leaves steps 1–4 at the old target; step 5 first uses the new target. Each update is `r_next = r + 0.2 * (delayed_target - r)`. Never apply the positive-flow division at zero flow. Display evidence must be matched to its underlying snapshot rather than an unrelated newer sample.

## Planned evidence package

For each check use `evidence/<run_id>/checks/<check_id>/` with the three filenames stated in its section. These directories/files are **planned outputs**, not existing links or fabricated artifacts. A fresh run and a unique check ID keep all 33 planned filenames distinct.

| Filename | Required content when executed |
| --- | --- |
| `source-records.jsonl` | Clearly labelled export/copy of the relevant original producer records. Preserve original IDs, source/sample/emission times and known action links; retain complete bounded windows when proving absence. Do not change the source schema or invent records/unknown actors. Preserve the original capture location in result.md. |
| `screen.png` | Actual FUXA/process-screen proof captured during the check, showing the relevant units and state. A diagram, configured switch or simulated screenshot alone is not observed runtime proof. |
| `result.md` | Check/story ID, run/scenario/settings/seeds, start/end steps, named actors and focal action, expected versus actual observations, original record IDs/times/locations, arithmetic comparisons, screenshot location, actual result, gaps and reviewer decision/name. Until execution, actual observations/evidence/reviewer confirmation are pending. |

No runtime record, screenshot, evidence directory or completed result is generated by this documentation task. A screenshot supports source records; it does not replace them or prove a missing approval/sample by itself.

## Coverage and initial runtime results

| Check | Story | Main observation | Runtime result | Reviewer |
| --- | --- | --- | --- | --- |
| AC-01 | US-01 | Observe ten baseline steps | Not run | Donald — pending |
| AC-02 | US-02 | Execute a different-user-approved change | Not run | Donald — pending |
| AC-03 | US-03 | Trace the labelled lab-unapproved change | Not run | Donald — pending |
| AC-04 | US-04 | Stop a pump after an approved setting | Not run | Donald — pending |
| AC-05 | US-05 | Suppress Sensor A for ten steps | Not run | Donald — pending |
| AC-04a | US-04 | Set flow to zero | Not run | Donald — pending |
| AC-04b | US-04 | Ignore an approved command | Not run | Donald — pending |
| AC-04c | US-04 | Reduce actual delivery by half | Not run | Donald — pending |
| AC-05a | US-05 | Emit Sensor A five seconds late | Not run | Donald — pending |
| AC-05b | US-05 | Hold A while model and B change | Not run | Donald — pending |
| AC-05c | US-05 | Bias only Sensor A | Not run | Donald — pending |

## AC-01 — Observe ten baseline steps

**Story:** US-01  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh baseline run: 10 L/s flow; requested and accepted feed 15 mg/s; pump running with delivery factor 1.0; demand 0.50 mg/L; modeled residual 1.00 mg/L; A/B normal with seeds 101/202.

**Setup:** Use the common reset/setup below. Emmanuel identifies model, equipment and A/B views and records the start step. No fabricated historical command/approval is needed to initialize the state.

**One focal action:** Emmanuel observes the next ten advancing simulation steps without changing the process.

**Observable expectations:** Flow remains 10 L/s and actual delivery 15 mg/s; modeled residual stays within the model tolerance of 1.00 mg/L. A and B produce separately identified samples within the raw-noise bound of model truth at each sample step. Times/sequences and plant steps demonstrate continuing operation. Displays distinguish settings, delivery and residual units. Initialized fields are not evidence of a prior request, approval, command or acknowledgement.

**Evidence filenames:** `evidence/<run_id>/checks/AC-01/source-records.jsonl`; `evidence/<run_id>/checks/AC-01/screen.png`; `evidence/<run_id>/checks/AC-01/result.md` — planned only.

**Evidence to retain:** Retain the run/scenario settings and start/end steps, model-state trace, equipment feedback and A/B source records; capture a screen with the separately labelled baseline quantities. Record actual observed bounds and source identities in result.md.

**Cleanup:** Preserve the completed run, leave all faults disabled and reset into a fresh run before another check.

## AC-02 — Execute a different-user-approved change

**Story:** US-02  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline with modeled residual 1.00 mg/L, feed 15 mg/s, positive 10 L/s flow and healthy pump; A/B normal.

**Setup:** Emmanuel creates a fresh request for 20 mg/s; Donald authorizes the matching request as a different named user. Retain actual actor/approval records. Define relative step 0 immediately before execution; the dose is applied before relative step 1.

**One focal action:** Execute that matching approved request through the normal desk/controller path once, then observe eight simulation updates.

**Observable expectations:** Preserve the request, approval, command and actual acknowledgement separately. With healthy equipment, accepted feed and actual delivery become 20 mg/s for step 1 onward. Modeled residual is 1.00 mg/L through steps 1–4, then 1.10, 1.18, 1.244 and 1.2952 mg/L at steps 5–8 within model tolerance; the target is 1.50 mg/L. Compare A/B with the water state at each source sample step, not directly with the 20 mg/s setting. An acknowledgement alone is insufficient proof of delivery.

**Evidence filenames:** `evidence/<run_id>/checks/AC-02/source-records.jsonl`; `evidence/<run_id>/checks/AC-02/screen.png`; `evidence/<run_id>/checks/AC-02/result.md` — planned only.

**Evidence to retain:** Retain the fresh request/approval actors, matching value/expiry where applicable, command, acknowledgement, equipment feedback and step-aligned model/A/B records. Preserve known action change IDs; sensor change_id normally stays null. Capture settings, delivery and delayed A/B response on the screen.

**Cleanup:** Preserve this action trail and run; clear pending commands through the documented reset before any next check.

## AC-03 — Trace the labelled lab-unapproved change

**Story:** US-03  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline on the isolated demonstration laptop; normal desk authorization remains required; no approval exists for the focal lab action.

**Setup:** Stephen selects the explicitly labelled local lab-unapproved teaching path. Keep its lab context distinguishable from ordinary desk execution. Establish step 0 at the baseline before invoking the action.

**One focal action:** Stephen invokes the lab-only feed change to 20 mg/s once without an approval.

**Observable expectations:** The intended lab example records the command and actual controller response; with healthy equipment it produces accepted feed and delivery of 20 mg/s followed by the same delayed water response described in AC-02. Approval is absent for this action: no approver or approval is invented. If this path is unavailable, rejected or behaves differently, retain that actual result rather than marking the intended lab outcome passed. Layer 1 preserves facts; later layers produce assurance verdicts.

**Evidence filenames:** `evidence/<run_id>/checks/AC-03/source-records.jsonl`; `evidence/<run_id>/checks/AC-03/screen.png`; `evidence/<run_id>/checks/AC-03/result.md` — planned only.

**Evidence to retain:** Retain Stephen's actual lab actor/context, command target, acknowledgement, feedback, model/A/B observations and the complete bounded action trail needed to demonstrate the approval gap. Do not invent a request event if the path did not produce one. Screen proof must visibly identify lab mode; a screenshot alone cannot prove absence of approval.

**Cleanup:** Disable the lab-unapproved path, preserve the run and reset to normal controls before the next check.

## AC-04 — Stop a pump after an approved setting

**Story:** US-04  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh run with a freshly approved 20 mg/s setting and healthy delivery, prepared using the settled-approved setup below; flow remains 10 L/s; A/B normal.

**Setup:** Emmanuel requests 20 mg/s and Donald approves; execute normally, advance 64 updates and confirm the model is within 0.000001 mg/L of 1.50 mg/L. Record the settled state and approval trail before the fault; if not settled, the precondition is unmet.

**One focal action:** Stephen selects the simulated stopped-pump fault once and observes the next eight updates.

**Observable expectations:** Accepted feed remains 20 mg/s while pump state becomes stopped and actual delivery zero mg/s. An earlier acknowledgement remains historical evidence and is not rewritten. At positive flow the new target is max(0, 0 / 10 - 0.50) = 0 mg/L. Four updates retain the prior delayed target; step 5 is the first using the zero target, with each update applying the documented 20% recurrence to the preceding model value. Sensor values follow the water response rather than becoming instant fresh zeros.

**Evidence filenames:** `evidence/<run_id>/checks/AC-04/source-records.jsonl`; `evidence/<run_id>/checks/AC-04/screen.png`; `evidence/<run_id>/checks/AC-04/result.md` — planned only.

**Evidence to retain:** Retain the fresh approved trail, pre-fault model state, fault action/context, acknowledgement separately from subsequent pump state/delivery feedback, eight model updates and A/B observations. Screen proof must show accepted 20 mg/s alongside stopped state and actual zero delivery.

**Cleanup:** Remove the stopped-pump fault, preserve the run and reset the baseline before another check.

## AC-05 — Suppress Sensor A for ten steps

**Story:** US-05  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline with a real recent A sample, continuing B samples and advancing plant steps.

**Setup:** Record A's last actual sample value, original UTC sample time and sequence, plus the starting plant step. Emmanuel observes the process while Stephen uses the source-specific lab control.

**One focal action:** Stephen selects missing-A mode for the next ten simulation steps, leaving B and plant operation enabled.

**Observable expectations:** No new A observations occur in that ten-step window; B observations and plant steps continue. If A's last value remains displayed, its original sample time is retained, age increases and an explicit unavailable indication is visible by the window's end. No new zero-valued A samples or invented records fill the gap. A connected device or refreshed screen is not evidence of fresh A data. The ten-step unavailability criterion is a DOS-4 proposal for review, not an already implemented timeout policy.

**Evidence filenames:** `evidence/<run_id>/checks/AC-05/source-records.jsonl`; `evidence/<run_id>/checks/AC-05/screen.png`; `evidence/<run_id>/checks/AC-05/result.md` — planned only.

**Evidence to retain:** Retain A's last real record, the bounded window of all source records showing no subsequent A samples, ongoing B records, plant start/end steps and available age/quality snapshots. Capture the unavailable-A indication while B/plant remain active.

**Cleanup:** Restore A to normal, retain the missing-data run and reset before the next check.

## AC-04a — Set flow to zero

**Story:** US-04  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline with 10 L/s flow, accepted feed 15 mg/s, healthy pump and both sources normal.

**Setup:** Record normal flow/delivery and the pre-action step. Identify the documented no-flow handling in the implemented model when it exists; do not substitute a tiny positive flow.

**One focal action:** Stephen sets simulated water flow to zero using the lab flow control.

**Observable expectations:** Flow becomes zero and actual chemical delivery becomes zero without changing the accepted setting merely to hide the condition. The calculation must take its no-flow handling rather than evaluate positive-flow division by zero; continuing ticks and absence of a division exception are required, with code-path/diagnostic review where needed. This check defines no specific zero-flow residual evolution because that policy is not yet specified; do not assert an instant zero concentration.

**Evidence filenames:** `evidence/<run_id>/checks/AC-04a/source-records.jsonl`; `evidence/<run_id>/checks/AC-04a/screen.png`; `evidence/<run_id>/checks/AC-04a/result.md` — planned only.

**Evidence to retain:** Retain the flow-change context, before/after flow and actual-delivery feedback, advancing steps and available model diagnostic or code-review reference for the no-flow branch. Capture zero flow/zero delivery and record the observed residual separately without imposing a new policy.

**Cleanup:** Restore positive flow, retain the run and reset before another check.

## AC-04b — Ignore an approved command

**Story:** US-04  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh baseline with accepted feed and actual delivery 15 mg/s, positive flow and normal sensors.

**Setup:** Stephen selects ignored-command mode before the focal execution. Emmanuel creates a fresh 20 mg/s request and Donald approves the matching request. Record the old accepted setting and new request separately.

**One focal action:** Execute that approved command once while ignored-command mode is selected, then observe eight updates.

**Observable expectations:** The requested target is 20 mg/s, but the accepted setting and healthy delivery retain their prior 15 mg/s values. Modeled residual remains at the baseline within tolerance. Preserve whatever acknowledgement the controller actually produces; do not invent a success or rejection status to fit the story. A command/approval record is not proof the requested value took effect.

**Evidence filenames:** `evidence/<run_id>/checks/AC-04b/source-records.jsonl`; `evidence/<run_id>/checks/AC-04b/screen.png`; `evidence/<run_id>/checks/AC-04b/result.md` — planned only.

**Evidence to retain:** Retain lab mode, fresh request/approval, command and actual acknowledgement alongside before/after accepted-feed, delivery, model and sensor records. Capture requested 20 mg/s and accepted/actual 15 mg/s together.

**Cleanup:** Disable ignored-command mode, preserve the run and reset before another check.

## AC-04c — Reduce actual delivery by half

**Story:** US-04  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh settled-approved 20 mg/s run at 10 L/s with normal A/B sources, using the 64-update setup.

**Setup:** Retain Emmanuel's fresh request, Donald's matching approval and the settled model/delivery state; keep the pump running.

**One focal action:** Stephen enables reduced delivery with factor 0.5 once and observes eight updates.

**Observable expectations:** Accepted feed remains 20 mg/s; actual delivery becomes 10 mg/s. The positive-flow target is max(0, 10 / 10 - 0.50) = 0.50 mg/L. The old target remains in the delay history for four updates, followed by 20% movement toward the new target per update. Compare with the captured preceding residual rather than assuming a rounded start value. Sensor data follow modeled water with source-specific noise.

**Evidence filenames:** `evidence/<run_id>/checks/AC-04c/source-records.jsonl`; `evidence/<run_id>/checks/AC-04c/screen.png`; `evidence/<run_id>/checks/AC-04c/result.md` — planned only.

**Evidence to retain:** Retain the fresh approved trail, factor-selection context, accepted feed, actual delivery, pre-fault and subsequent model values and separate A/B observations. Capture accepted 20 mg/s versus actual 10 mg/s and the delayed response.

**Cleanup:** Restore delivery factor 1.0, preserve the run and reset before another check.

## AC-05a — Emit Sensor A five seconds late

**Story:** US-05  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline with A/B normal. Run at one simulation step per real second without pausing for this timestamp comparison.

**Setup:** Stephen configures A's five-second delay mode; keep B normal. Do not rewrite original sample times when buffering observations.

**One focal action:** Enable the configured A delay and observe twelve real-time steps, comparing delayed A records with model truth at their original sample steps.

**Observable expectations:** A observations retain their original source_timestamp_utc and sim_step; emitted_at_utc is 5–6 seconds later under the proposed one-tick scheduling allowance. Emission before five seconds or after six seconds fails this proposed timing criterion; record environmental/prerequisite issues rather than silently widening it. B remains current. Arrival/collection time cannot replace sample time, and apparent device connectivity does not prove A freshness.

**Evidence filenames:** `evidence/<run_id>/checks/AC-05a/source-records.jsonl`; `evidence/<run_id>/checks/AC-05a/screen.png`; `evidence/<run_id>/checks/AC-05a/result.md` — planned only.

**Evidence to retain:** Retain A sample/emission timestamp pairs, values, original sample steps and corresponding model output, continuing B records and the twelve-step window. Capture source age/quality context that distinguishes late A from current B.

**Cleanup:** Disable A delay, clear delayed-sample queues through reset and preserve the completed run.

## AC-05b — Hold A while model and B change

**Story:** US-05  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline with a recent actual A value and positive flow; B normal.

**Setup:** Stephen selects stuck-A mode and records the value being held. Emmanuel prepares a fresh 20 mg/s request and Donald approves it; define relative step 0 before execution.

**One focal action:** Execute the approved change once with A already held, then observe eight updates.

**Observable expectations:** Model truth follows AC-02's delayed response and B follows that changing water within its normal noise bound. A continues the selected stuck-mode observations with the held value while sequence/sample time advance in this proposed example. Advancing metadata or apparently good quality alone cannot prove the reading is correct. Compare the held value with changed model/B data rather than testing stuck A only against an unchanged baseline.

**Evidence filenames:** `evidence/<run_id>/checks/AC-05b/source-records.jsonl`; `evidence/<run_id>/checks/AC-05b/screen.png`; `evidence/<run_id>/checks/AC-05b/result.md` — planned only.

**Evidence to retain:** Retain the held-value record, fault context, new action trail, repeated A values with advancing source metadata and step-aligned model/B observations. Capture the diverging A/B trend and model comparison.

**Cleanup:** Restore A to normal, preserve the run and reset before another check.

## AC-05c — Bias only Sensor A

**Story:** US-05  
**Runtime result:** Not run. **Actual observations:** Pending execution. **Reviewer:** Donald — pending.

**Starting conditions:** Fresh normal baseline with A/B identities and original source metadata distinguishable; no other selected fault.

**Setup:** Identify the modeled residual and both source readings at matching sample steps. Keep B normal; Stephen uses the source-specific bias control.

**One focal action:** Stephen enables a +0.40 mg/L bias on A and observes ten steps.

**Observable expectations:** Raw A minus the corresponding modeled residual is between +0.39 and +0.41 mg/L including the normal ±0.01 noise; raw B remains within ±0.01 mg/L of model truth. Apply display-quantization allowance separately when reading the screen. A/B disagreement is evidence to investigate, not proof that physically independent instruments exist or that B guarantees correctness.

**Evidence filenames:** `evidence/<run_id>/checks/AC-05c/source-records.jsonl`; `evidence/<run_id>/checks/AC-05c/screen.png`; `evidence/<run_id>/checks/AC-05c/result.md` — planned only.

**Evidence to retain:** Retain the selected offset/source context, matching model snapshots, raw A/B values with identities/times/quality and ten-step window. Capture labelled A/B readings or trends and record the measured differences in result.md.

**Cleanup:** Remove A's bias, preserve the run and reset before another check.

## Documentation review and board handoff

| Named check | Person | State |
| --- | --- | --- |
| Calculation, timing, 64-step setup and proposed tolerances | Katekiso | Pending. |
| Source identity, units, timestamps, gaps and evidence completeness | Emmanuel | Pending. |
| Actors, approval boundaries and documentation acceptance | Donald | Pending. |
| Controller feedback and no-flow/ignored-command interface expectations | Stephen | Pending. |
| Display bindings, scale and age/unavailable screen proof | Tranos | Pending. |
| Integration, coverage and complete agreed check definitions | Thabani | Pending. |

At preparation on 2026-10-27, Review contained four cards (DOS-1, DOS-2, DOS-3 and DOS-7), above the proposed three-card limit. DOS-4 remains **In Progress** with documentation prepared and awaiting review capacity. Before moving it to Review, check that fewer than three cards already occupy that column. Do not move other cards or grant an automatic exception to make room.

DOS-4 documentation acceptance means the team agrees these definitions are usable. It does not require or claim runtime passes before implementation. Record real findings, resolve required corrections and reconcile the pending DOS-1/DOS-2/DOS-3 inputs before acceptance. The [acceptance register](acceptance.md) keeps DOS-4 documentation and parent WBS 1.1 pending; future runtime results remain Not run until actual execution. Assurance verdicts belong to later DoseProof layers, which receive copies and do not send dosing commands back.
