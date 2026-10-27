# DOS-3: Five Layer 1 simulation stories

**WBS:** 1.1.3 | **Owner:** Thabani | **Reviewer:** Donald  
**Status:** Prepared for review; acceptance pending.  
**Contributor checks:** Stephen — control/equipment meanings; Donald — actors/approval and acceptance. Both pending.

> **Proposed stories, not execution evidence.** These educational examples describe a software-only plant on one laptop. They do not claim a running simulation, configured accounts, completed actions or human approval. All numerical values are teaching choices, not real treatment operating limits or proof of drinking-water safety.

## How to read these stories

DOS-3 describes five small pieces of intended behaviour. Each story has a stable ID for DOS-4 to reference. The action summaries and expected outcomes below are story-level requirements, not executable test procedures or completed acceptance checks. DOS-4 owns the detailed starting conditions, actions, timing/tolerances and evidence checks; later Layer 1 tasks implement and demonstrate them.

| Story | Main example |
| --- | --- |
| US-01 | Normal baseline operation |
| US-02 | Different-user-approved change |
| US-03 | Explicitly labelled lab-unapproved change |
| US-04 | Stopped dosing pump despite an approved setting |
| US-05 | Missing Sensor A while B and the plant continue |

**Illustrative actors:** Emmanuel is the requester and observer; Donald is the different-user approver; Stephen operates lab fault/scenario controls. These proposed scenario assignments do not establish real accounts, permissions, actions or sign-off. Donald's illustrative in-story approval is separate from his still-pending review of this document.

**Pending inputs:** [DOS-1 / PR #1](https://github.com/thamas97/doseproof/pull/1) supplies the treatment-line scope; [DOS-2 / PR #2](https://github.com/thamas97/doseproof/pull/2) explains the teaching settings and timing. Both remain under review. This independent branch starts from main, does not import their unmerged files and does not accept their assumptions on the team's behalf. Reconcile relevant feedback before accepting dependent stories.

The existing [architecture](architecture.md), [baseline scenario](../scenarios/baseline.json), [register map](register-map.md), [event contract](event-schema.md) and [runbook](reference/runbook.txt) remain unchanged design references. At baseline: 10 L/s flow, 15 mg/s requested/accepted feed, a healthy pump delivering 15 mg/s, fixed demand 0.50 mg/L and modeled residual 1.00 mg/L. Sensors A/B observe water residual in mg/L with separate source IDs/seeds and noise bounded at ±0.01 mg/L.

## US-01 — Observe normal operation

**Story:** As Emmanuel, the process observer, I want to see the normal water-treatment state so that I can understand the baseline before interpreting a change or fault.

**Named actors:** Emmanuel observes. No new requester/approver action is needed just to observe initialized state.

**Action summary:** Observe the initialized process with positive water flow, a running healthy dosing pump and both sensor sources operating normally. Compare separately labelled settings, actual delivery and A/B observations.

**Expected observable outcome:** Requested and accepted feed initialize at 15 mg/s; actual delivery is 15 mg/s. Modeled residual remains 1.00 mg/L at the settled baseline because `max(0, 15 / 10 - 0.50) = 1.00`. A/B readings may differ slightly because of their noise. A fresh measurement is not the same evidence item as a feed setting.

**Records needed to explain it:** Run/scenario identity and simulation steps, equipment feedback and separately identified A/B observations with source identity, sample time, value, unit and quality. Initialization is not historical approval evidence: do not fabricate a prior request, approval, command or acknowledgement to explain the starting fields.

## US-02 — Make an approved change

**Story:** As Emmanuel, the requester, I want a different named user to approve my dosing request so that the authorized change and its actual process response can be traced separately.

**Named actors:** Emmanuel requests; Donald approves the matching request as a different user. The software controller executes the approved path and records its response.

**Action summary:** From the baseline proposal, Emmanuel requests 20 mg/s. Donald authorizes that matching request. The controller handles the command; the pump/model reports actual delivery, and the sensors subsequently observe the modeled water.

**Expected observable outcome:** Requested feed, approval, accepted feed, acknowledgement, actual delivery and measurements remain distinct. With valid approval and a healthy pump, accepted feed and actual delivery become 20 mg/s. The target modeled residual is `max(0, 20 / 10 - 0.50) = 1.50 mg/L`; water response is delayed rather than instantaneous. Using the pending DOS-2 convention, a change before step 1 leaves residual unchanged through steps 1–4, with the first adjustment at step 5. An acknowledgement alone does not prove delivery or correct sensor observations.

**Records needed to explain it:** Request and approval actors, matching target and applicable approval expiry, command, acknowledgement and equipment feedback; preserve the shared `change_id` along the action trail where known. Retain separate A/B source records. Do not automatically give a sensor sample the command's change ID merely because it follows the command; sensor `change_id` is normally null under the existing contract.

## US-03 — Trace a lab-unapproved change

**Story:** As Stephen, the lab scenario operator, I want an explicitly labelled local unapproved-change example so that later DoseProof monitoring can distinguish a process change with no approval from an ordinary approved change.

**Named actors:** Stephen invokes the labelled lab-only path. There is no approver for this action; do not imply that Donald approved it.

**Action summary:** In the isolated software demonstration, Stephen deliberately selects the labelled lab-unapproved action to change the feed to 20 mg/s without an approval. This special teaching path is separate from normal desk execution, whose authorization checks remain required. Return the lab controls to their normal condition after the example; DOS-4 defines the detailed procedure.

**Expected observable outcome:** The intended lab example records an accepted command and, with a healthy pump and positive flow, actual delivery of 20 mg/s followed by the modeled delayed water response. The distinguishing fact is the absent approval, not a fabricated rejection or a missing actor trail. The source records preserve what actually happens when implemented; this story is not a claim that the path already works. Layer 1 records the facts; later layers evaluate approval evidence without sending dosing commands back.

**Records needed to explain it:** Stephen's actor identity and lab context, command target, acknowledgement, actual equipment feedback and A/B observations. Preserve known action IDs and times. Approval is absent; use the existing contract's explicit unknown/absence explanation rather than inventing an approver or approval. Do not invent a request record if that lab path did not produce one.

## US-04 — Notice equipment failure after approval

**Story:** As Emmanuel, the observer, I want to distinguish an approved setting from a stopped dosing pump so that a successful command response is not mistaken for continued chemical delivery.

**Named actors:** Emmanuel requests 20 mg/s and Donald authorizes the matching request in this proposed story. Stephen selects the simulated stopped-pump fault. These are illustrative actions, not a dependency on actually executing US-02 first.

**Action summary:** Consider a healthy approved setting of 20 mg/s at positive 10 L/s flow. Then Stephen stops the virtual dosing pump while that accepted setting remains recorded.

**Expected observable outcome:** Accepted feed remains 20 mg/s while pump state becomes stopped and actual delivery becomes zero mg/s. An earlier acceptance acknowledgement remains historical command evidence; it does not prove the pump is still delivering. Under the teaching calculation, the zero-delivery target at positive flow is zero mg/L, reached gradually through the modeled response rather than by instantly replacing sensor values with zero. A/B observations follow the water state, subject to their normal source behaviour.

**Records needed to explain it:** The fresh request/approval/action trail for this story, the acknowledgement separately from subsequent equipment feedback, pump state, actual delivery, modeled-state comparison when available and separate sensor observations. Keep the lab fault context and original timestamps interpretable; do not rewrite an earlier acknowledgement to conceal the later fault.

## US-05 — Notice unreliable monitoring

**Story:** As Emmanuel, the observer, I want to see when Sensor A stops reporting while Sensor B and the plant continue so that an old retained value is not mistaken for a fresh measurement.

**Named actors:** Stephen selects the missing-A simulation mode; Emmanuel observes the result. No dosing approval is implied by this sensor-fault selection.

**Action summary:** With normal plant operation and both sources initially present, Stephen makes A stop producing samples while B continues sampling the same modeled water.

**Expected observable outcome:** Plant steps and B observations continue. A's last value, if retained for display, becomes old/unavailable with age/quality context rather than a fresh zero. Device connectivity alone is not proof of A's freshness. Both sources still share one software model and laptop: separate source IDs and seeds permit distinct simulated faults, not physical independence or proof that B is correct.

**Records needed to explain it:** The selected fault/source context, A's last real sample and original sample time, absence of subsequent A samples, available source-age/quality indications, ongoing B samples and advancing plant steps. Record gaps explicitly; do not fabricate observations to fill them. Collection timestamps or repeated displays must not replace the original sample time.

## Handoff to DOS-4 and later implementation

Keep these five IDs stable. DOS-4 will define detailed starting conditions, actions, expected observations, timing/tolerances and evidence checks for the stories, rather than treating these descriptions as passed tests.

Expand equipment variants separately: **no flow** (zero actual delivery without division by zero), **ignored command** (retain the previous accepted setting), and **reduced delivery** (factor 0.5). Expand sensor variants separately on A while B continues: **delayed** (five seconds late with original sample time), **stuck** (held value even if time/sequence advances), and **biased** (+0.40 mg/L). These are coverage notes, not six extra stories or implemented procedures. Change one fault at a time in the eventual checks.

No runtime code, public API, event schema, register address or saved scenario setting is introduced or changed here. Exact event-type names and event-specific fields remain governed by the existing contract. FUXA is an observer; execution remains in the desk/controller path. Assurance verdicts and alerts belong to later DoseProof layers, not these Layer 1 stories.

## Review record

| Check | Person | State |
| --- | --- | --- |
| Prepare the five stories and preserve scope boundaries | Thabani | Prepared for review; no team sign-off implied. |
| Check request/accepted setting, acknowledgement, equipment outcome and measurement distinctions | Stephen | Pending. |
| Check named actors, different-user approval and the separate labelled lab-unapproved path | Donald | Pending. |
| Confirm the five stories satisfy DOS-3 / WBS 1.1.3 | Donald | Pending. |

Reconcile relevant DOS-1/DOS-2 feedback, record the named findings and resolve required corrections before accepting DOS-3. The [acceptance register](acceptance.md) keeps DOS-3 and parent WBS 1.1 pending. Detailed DOS-4 checks and all live evidence remain separate unfinished work.
