# DOS-1 Treatment-line scope

**WBS:** 1.1.1 | **Owner:** Thabani | **Reviewer:** Donald  
**Status:** Prepared for review; team acceptance pending.  
**Contributors:** Tranos checks display feasibility; Stephen checks control meanings.

This document describes the design of one software-only chlorine-treatment line. It does not claim that the plant, desk, sensors or FUXA bindings have been implemented or demonstrated. All components share one isolated demonstration laptop. The values and behaviour in the linked contracts are proposed teaching choices, not real treatment operating limits.

## Follow the treatment line

![DoseProof treatment-line design: inlet water passes the chlorine injection point, mixes in the contact tank and reaches the outlet; sensors A and B separately sample the same modeled water state. Separate control, feedback and copied-record paths are labelled.](process-sketch.png)

[Open the editable SVG](process-sketch.svg) | [Open the full-size PNG](process-sketch.png)

The water path is **inlet -> flow -> chlorine injection -> mixing/contact tank -> outlet**. The dosing pump supplies chlorine into the water line; it is not the water-inlet pump. Both residual sensors sample the modeled water after the tank. They are parallel observation paths, not two treatment stages.

| Component | What it represents | What to label or observe |
| --- | --- | --- |
| Water inlet and flow | Water entering the teaching process. The model provides the flow used in the calculation. | Water flow in L/s. |
| Chlorine dosing pump | Chemical addition to the water. A request to run the pump is different from what it delivers. | Requested feed, accepted feed and actual delivery in mg/s; pump state. |
| Mixing/contact tank | The modeled water state after mixing, demand and the proposed response delay. | Modeled residual chlorine in mg/L. |
| Water outlet | The water state leaving this modeled treatment stage. | A clear end to the water path; no real water is handled. |
| Sensor A and sensor B | Separate software adapters sampling the same modeled residual. They can later have distinct noise and fault settings. | Each source's residual in mg/L, identity, sample time and quality. |

## Keep five meanings separate

| Evidence item | Meaning | What it does not prove |
| --- | --- | --- |
| Requested feed | The chlorine-feed value the named requester asks for. | It has been approved or applied. |
| Approval and accepted feed | A different named user authorizes the matching request; the controller records the setting it accepts. An approved request can still fail at execution. | Actual chemical delivery or the resulting water state. |
| Acknowledgement | The controller's recorded response to the command, including acceptance or rejection. | That the pump delivered the accepted amount. |
| Actual delivery | The pump/model feedback describing simulated chlorine feed delivered, in mg/s. | That the sensors observed a fresh, correct residual. |
| Water state and measurements | The model calculates water residual; A and B separately observe that state, in mg/L, with their own source metadata. | That both sources are physically independent or that real water is safe. |

An approved command should produce a traceable controller response, equipment feedback and later sensor observations. The exact teaching values, timing and expected outcomes remain subject to DOS-2, DOS-3 and DOS-4. This document introduces no new calculation, event schema or register address.

## Control and observation boundaries

- **Operator desk:** a named user requests a change and a different named user approves it. Execution belongs in this desk and the controller path, not in the evidence dashboard.
- **Software controller:** keeps request, accepted setting, acknowledgement and actual delivery distinguishable. Detailed command and engine behaviour remains in the existing architecture proposal.
- **FUXA:** displays read-only controller snapshots for flow, settings, actual delivery, pump state and A/B readings. Server-side observer write restrictions still need implementation and testing; a diagram or missing button is not proof of enforcement.
- **Sensor adapters:** read the modeled water state, not a controller setpoint or the FUXA display. Distinct IDs and seeds support separate software fault cases; the shared model and laptop remain common failure points.
- **Future DoseProof layers 2-8:** receive copied desk, controller/equipment and sensor records. They assess evidence and do not send dosing commands back. The copy arrows in the sketch describe the intended handoff, not an implemented connector.

## Scope and handoffs

Included here: one chemical, one dosing pump, one contact tank, one water-flow path, two residual-sensor paths, a read-only process view, and the control/evidence distinctions needed to discuss them.

The existing [architecture](architecture.md), [register map](register-map.md) and [event contract](event-schema.md) remain the detailed design references. No API, schema or register-map interface changes are made by DOS-1.

| Follow-on card | Owns the remaining work |
| --- | --- |
| DOS-2 / WBS 1.1.2 | Agree teaching assumptions, units, initial state, timing and pause/reset meanings. |
| DOS-3 / WBS 1.1.3 | Write the five normal/approved/unapproved/equipment-fault/unreliable-monitoring stories. |
| DOS-4 / WBS 1.1.4 | Define each story's starting conditions, action, expected observation and evidence. |
| Later Layer 1 tasks | Implement the model, desk, controller, sensors, FUXA bindings, source records and live checks. |
| Layers 2-8 | Implement collection, validation, correlation, assurance rules, evidence storage, API and dashboard. |

## Review record

Documentation checks can confirm the drawing is readable and consistent with the proposed contracts. They cannot establish live plant behaviour or substitute for the named team's review.

| Check | Person | State |
| --- | --- | --- |
| Scope text and the canonical sketch prepared | Thabani, accountable owner | Prepared for review; no team sign-off implied. |
| Labels and symbols are feasible for a beginner FUXA process screen | Tranos | Pending. |
| Request, approval, accepted setting, acknowledgement, delivery and observations have the correct meanings | Stephen | Pending. |
| Confirm the description and sketch satisfy DOS-1 | Donald | Pending. |

Acceptance requires one agreed sketch showing inlet/flow, chemical addition, tank, outlet and A/B sources with settings, delivery and measurements separately labelled. Record the named checks before closing DOS-1. The parent WBS 1.1 acceptance remains pending because DOS-2 through DOS-4 are separate unfinished work.
