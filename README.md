# DoseProof

DoseProof Layer 1 is a software-only water-treatment plant for demonstrating the difference between a requested dose, an approved command, actual pump delivery, and two sensor observations.

This repository starts the team workspace described in the **DoseProof Layer 1 All Member Task Runbook**. It contains the project structure, proposed interface contracts, scenario settings, and all 36 WBS tasks. The plant application, authenticated desk, Modbus service, and FUXA screen still need to be implemented. All live acceptance checks are pending.

The [DOS-3 simulation stories](docs/story-checks.md) describe normal, approved, lab-unapproved, equipment-fault and unreliable-monitoring behaviour. They are prepared for review; detailed DOS-4 checks and live evidence remain pending.

## Scope

- One teaching model: water inlet, dosing pump, contact tank, and sensors A/B.
- Named operators request changes; a different named user approves them.
- Command acknowledgement is separate from equipment feedback.
- Equipment and sensor fault scenarios retain traceable source records.
- FUXA reads plant snapshots; command execution belongs in the desk.
- Later DoseProof layers consume source records without owning plant state.

These values describe a teaching simulation and must not be used as real treatment operating limits.

## Start here

1. Read [scope and architecture](docs/architecture.md) and the [source runbook](docs/reference/runbook.txt).
2. Confirm the proposed [register map](docs/register-map.md), [event contract](docs/event-schema.md), and [baseline scenario](scenarios/baseline.json).
3. Choose a pending WBS item in the [task board](docs/task-board.csv). Every member contributes to every main task.
4. Follow [CONTRIBUTING.md](CONTRIBUTING.md), save an observed result, and request peer review.
5. Record acceptance in [docs/acceptance.md](docs/acceptance.md). A file or configured switch alone does not demonstrate live behavior.

## Repository layout

```text
src/doseproof/        Planned plant modules and application entry point
tests/               Future behavioral and integration checks
scenarios/           Saved simulation parameters and seeds
fuxa/                Future saved FUXA configuration exports
docs/                Contracts, setup plan, acceptance, and WBS board
docs/reference/      Text extracted from the supplied runbook
source-examples/      Illustrative fixtures, distinct from captured records
evidence/            Locally captured runtime records, ignored by Git
.github/             Issue and pull request templates
```

## Development status

There is no runnable application yet. [docs/setup.md](docs/setup.md) records the proposed stack and service order, including what must be verified before startup instructions become executable. No dependency lock, FUXA export, captured runtime evidence, or completed acceptance result has been fabricated.

## Team and schedule

| Member | Layer 1 focus |
| --- | --- |
| Thabani | Scope and integration acceptance |
| Tranos | FUXA setup and process screen |
| Emmanuel | Sensor sources and event examples |
| Katekiso | Teaching model and expected outcomes |
| Stephen | Virtual controller and command interface |
| Donald | Authenticated request and approval desk |

The plan uses five working-day positions, one hour per person per day: 30 team-hours, including 20.5 hours of assigned work and 9.5 hours of coordination, review, updates, and reserve. Calendar dates and completion remain unconfirmed.

## License

No open-source license has been selected. Repository creation does not grant a license to reuse its contents.
