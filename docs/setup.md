# Setup and startup plan

Status: pending implementation and environment verification. This repository does not yet provide `app.py` or a runnable plant.

## Proposed stack from the runbook

| Component | Proposed version or package |
| --- | --- |
| Python | 3.12; earlier plan referenced 3.12.10 |
| Node.js | Earlier plan referenced 24.14.1 |
| Modbus | `pymodbus==3.16.1` |
| FUXA | `@frangoteam/fuxa@1.3.4` |
| Desk | Flask and Flask-WTF; versions to be selected and locked |

These are source-plan values, not verified compatibility claims or installed versions. Confirm availability and APIs in official documentation, create a local virtual environment, record actual versions in `versions.txt`, and commit `requirements.lock.txt` and `package-lock.json` after a successful setup. On PowerShell, use `npm.cmd` if script policy blocks `npm.ps1`.

## Planned local services

| Service | Interface |
| --- | --- |
| Modbus observer | `127.0.0.1:1502`, device 1, Holding Registers |
| Authenticated desk | `127.0.0.1:5000` |
| FUXA | Local port 1881, 1000 ms polling |

After implementation, start the plant once, confirm ticks and source records, start FUXA with the same saved state folder, then inspect desk and observer independently. Save FUXA state and export JSON before stopping. Use Ctrl+C only in this project's service windows. Report port conflicts instead of terminating unrelated programs.

## Startup and reset proof to add

- Exact verified install and launch commands, dependency locks, and installed versions.
- One changing reading in FUXA, correct scale, and restart from saved state.
- Named-user setup without committed credentials.
- Reset instructions and a replay comparison by simulation step.
- Independent startup by another member, including observer disconnection while ticks continue.

Keep these checks pending until observed results are retained.
