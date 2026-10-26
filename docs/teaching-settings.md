# DOS-2: Simulation teaching settings

**WBS:** 1.1.2 | **Owner:** Thabani | **Reviewer:** Donald  
**Status:** Prepared for review; team agreement pending.  
**Contributor checks:** Katekiso — calculation/timing; Emmanuel — units and source metadata. Both pending.

> **Teaching simulation only.** Every number below is an educational simulation choice, not a real treatment operating limit, drinking-water safety threshold or engineering design recommendation. These are documented expectations, not captured runtime evidence. No running plant or live acceptance is claimed.

## 1. What this document specifies for review

This specification retains the baseline in [baseline.json](../scenarios/baseline.json) and section 3 of the [source runbook](reference/runbook.txt). Read it alongside the [architecture](architecture.md), [observer register map](register-map.md) and [source event contract](event-schema.md). Those interfaces remain unchanged.

[DOS-1's treatment-line sketch and description](https://github.com/thamas97/doseproof/pull/1) are a preceding scope input still awaiting review. This independent DOS-2 branch does not import or accept that unmerged work. Reconcile any later DOS-1 scope corrections before accepting dependent assumptions.

DOS-2 provides the settings, calculation examples and pause/reset meanings for the team to review. DOS-3 owns the five detailed stories; DOS-4 owns their starting conditions, actions, expected observations and evidence checks. Later Layer 1 tasks implement and demonstrate the behaviour. Layers 2–8 collect and assess copied records; they do not send dosing commands back.

## 2. Keep the meanings separate

| Term | Beginner explanation | Unit or evidence |
| --- | --- | --- |
| Requested feed | What a named requester asks the chlorine pump to deliver. A request alone is not permission to execute. | mg/s |
| Approval | A different named user authorizes the matching request. Approval does not prove execution. | Actor and approval record |
| Accepted feed | The setting the controller accepts. It can differ from the latest requested value. | mg/s |
| Acknowledgement | The controller's response, including acceptance or rejection. It is not delivery feedback. | Command/response record |
| Actual delivery | What the simulated pump/model reports as delivered chemical. A fault can make this lower than the accepted setting. | mg/s |
| Modeled residual | Chlorine concentration calculated for the simulated water after the teaching demand and delayed response. | mg/L |
| Sensor measurement | What source A or B observes from that modeled water, with source-specific noise, faults and metadata. | mg/L, source identity, sample time, quality |
| Water flow | Water volume passing through the simulated line each second. | L/s |

mg means milligrams; L means litres; s means seconds. A feed rate in mg/s is not the same quantity as a residual concentration in mg/L. The water calculation uses **actual delivery**, not the requested feed or acknowledgement.

## 3. Baseline: existing simulation choices

Every row is a teaching choice already present in the saved scenario. The scenario retains its `proposed` status until the appropriate agreement is recorded; its keys and values are not changed by this task.

| Existing scenario field | Teaching value | Meaning |
| --- | --- | --- |
| `tick_seconds` | 1 second | One simulation step represents one second. |
| `flow_l_s` | 10.0 L/s | Baseline water flow. |
| `requested_feed_mg_s` | 15.0 mg/s | Initial requested-feed display/state. |
| `accepted_feed_mg_s` | 15.0 mg/s | Initial accepted-feed state. |
| `pump_running` | true | Pump starts running in this scenario. |
| `delivery_factor` | 1.0 | Healthy pump delivers the accepted feed, subject to flow/pump state. |
| `demand_mg_l` | 0.50 mg/L | Fixed subtraction in the toy calculation. |
| `initial_residual_mg_l` | 1.00 mg/L | Starting modeled residual. |
| `response_delay_ticks` | 4 steps | Hold the old target for four updates after the example change. |
| `response_fraction_per_tick` | 0.2 (20%) | Move one fifth of the remaining difference per update. |
| `sensor_a.source_id`, `sensor_a.seed` | `sensor-a`, 101 | A's identity and separate random-generator seed. |
| `sensor_b.source_id`, `sensor_b.seed` | `sensor-b`, 202 | B's identity and separate random-generator seed. |
| `sensor_a.noise_bound_mg_l`, `sensor_b.noise_bound_mg_l` | ±0.01 mg/L each | Bounded normal-mode noise; not a physical accuracy guarantee. |
| `sensor_a.mode`, `sensor_b.mode` | `normal`, `normal` | No selected sensor fault at baseline. |
| `fault_options.sensor_delay_seconds` | 5 seconds | Additional late-emission fault for a selected sensor. |
| `fault_options.sensor_bias_mg_l` | +0.40 mg/L | Positive offset fault for a selected sensor. |
| `fault_options.reduced_delivery_factor` | 0.5 | Impaired delivery is half the accepted feed. |

At healthy baseline, actual delivery is 15 mg/s. Initializing requested/accepted fields at 15 mg/s **does not constitute a captured request, approval, command or acknowledgement**. Do not fabricate historical actors or records to explain initialized state. A later approved action needs its own traceable records.

The four-step water-response delay and the five-second delayed-sensor fault are different: one changes when the modeled water responds; the other makes an observation arrive late while retaining its original sample time.

## 4. Worked water-response calculation

For **positive flow only**, calculate:

```text
target residual (mg/L) = max(0, actual delivery (mg/s) / flow (L/s) - demand (mg/L))
next residual = current residual + 0.2 * (delayed target - current residual)
```

The unit check is `(mg/s) / (L/s) = mg/L`. The `max(0, ...)` prevents a negative target. This simplified calculation and artificial delay illustrate cause and effect; they do not model all treatment chemistry or validate real disinfection.

**Baseline:** `max(0, 15 / 10 - 0.50) = 1.00 mg/L`. The initialized residual is already 1.00 mg/L, so a normal undisturbed model remains there. Sensor noise can make A/B readings differ slightly from model truth.

**Approved-step example:** assume a fresh, different-user-approved request is accepted at 20 mg/s and a healthy pump delivers 20 mg/s. The new target is `max(0, 20 / 10 - 0.50) = 1.50 mg/L`. The command response and delivery change precede the delayed water response; an acknowledgement alone never proves this result.

### Exact teaching delay convention

Step 0 is the settled baseline. Apply the example change after step 0 and before step 1. Actual delivery is 20 mg/s for step 1 onward. Steps 1–4 still use the previous 1.00 mg/L target; step 5 is the first update using the new 1.50 mg/L target. At initialization/reset the delay history represents the baseline target, avoiding an artificial startup dip.

| Simulation step | Actual delivery (mg/s) | Target used for this update (mg/L) | Expected modeled residual (mg/L) |
| --- | --- | --- | --- |
| 0 — before change | 15 | 1.00 | 1.00 |
| 1 | 20 | 1.00 | 1.00 |
| 2 | 20 | 1.00 | 1.00 |
| 3 | 20 | 1.00 | 1.00 |
| 4 | 20 | 1.00 | 1.00 |
| 5 | 20 | 1.50 | 1.10 |
| 6 | 20 | 1.50 | 1.18 |
| 7 | 20 | 1.50 | 1.244 |
| 8 | 20 | 1.50 | 1.2952 |

For example, step 5 is `1.00 + 0.2 * (1.50 - 1.00) = 1.10`; step 6 is `1.10 + 0.2 * (1.50 - 1.10) = 1.18`. Later updates approach 1.50; they do not jump there immediately.

**These are calculated, noise-free model expectations.** They are neither exact A/B sensor outputs nor captured plant measurements. Compare unrounded model values when implementing; display/register rounding and source metadata remain governed by their existing contracts.

## 5. Pause, resume, reset and replay

| Operation | Proposed meaning for review | Evidence implication |
| --- | --- | --- |
| Pause | Freeze simulation steps, modeled state and model/delay advancement. | Pausing does not create evidence that the water reached a new target. UTC time remains real time. |
| Resume | Continue from the retained simulation state and delay position. | Continue the same run; retain the original source times of existing observations. |
| Reset | Restore saved baseline state, pump/settings and both seeds; clear pending commands, delayed-sample queues and active faults; reinitialize baseline delay history. | Create a fresh run ID, preserve previous files and do not replay queued commands from the old run. |
| Replay | Use the same saved settings, seeds and action timeline. | Compare model/sensor values by simulation step. Fresh run/event IDs and different UTC timestamps are expected. |

These lifecycle meanings guide later implementation. This task adds no lifecycle endpoint, new scenario key or timer API. Live pause/reset/replay tests still need implementation and observation; their absence is not concealed by this document.

## 6. Equipment and sensor interpretation

**Equipment:** no flow means zero actual delivery and the positive-flow division must not be evaluated. Do not substitute a tiny flow and imply a valid concentration. A stopped pump also delivers zero. With positive flow, reduced mode gives `accepted feed * 0.5`; an ignored command leaves the previous accepted setting in place. Keep requests, accepted settings, acknowledgements and actual feedback separate in all cases. Detailed fault stories and observed checks belong to DOS-3/DOS-4 and later model/controller work.

**Sources A and B:** both sample the modeled water residual, not a controller setting or the FUXA display. Keep each source's ID, sequence, sample time, value, unit and quality distinguishable. A delayed observation keeps its original sample UTC time and has a later emission time; arrival time is not sample time. A stuck value can still have advancing timestamps, so apparent freshness alone does not prove correctness.

Missing samples must not become a fresh zero. A retained value needs its source age/quality context; unknown fields follow the existing event contract rather than invented data. Both sensors share one model and laptop. Separate seeds/source IDs permit distinct software faults, not physical independence or proof that both cannot fail together.

## 7. Review and acceptance record

| Check | Person | State |
| --- | --- | --- |
| Prepare the document and preserve scope/interface boundaries | Thabani | Prepared for review; no team sign-off implied. |
| Check toy calculation, four-step delay, initial state and replay expectations | Katekiso | Pending. |
| Check units, sensor identity, sample/emission times and missing-data meaning | Emmanuel | Pending. |
| Confirm DOS-2's assumptions and lifecycle meanings satisfy WBS 1.1.2 | Donald | Pending. |

Record each named finding, resolve required corrections and reconcile the pending DOS-1 scope input before accepting DOS-2. The [acceptance register](acceptance.md) remains pending for this task and parent WBS 1.1. DOS-2 must not be marked Done merely because this specification or a scenario file exists.
