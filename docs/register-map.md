# Proposed observer register map

All fields are UInt16 Holding Registers. Scaling is applied once. FUXA address conversion below is a proposal transcribed from the runbook and must be verified against the installed version before relying on it.

| Protocol address | FUXA UI address | Snapshot | Decode |
| --- | --- | --- | --- |
| 0 | 1 | Sensor A residual | Divide by 100 for mg/L |
| 1 | 2 | Sensor B residual | Divide by 100 for mg/L |
| 2 | 3 | Flow | Divide by 100 for L/s |
| 3 | 4 | Requested feed | Divide by 100 for mg/s |
| 4 | 5 | Accepted feed | Divide by 100 for mg/s |
| 5 | 6 | Actual delivery | Divide by 100 for mg/s |
| 6 | 7 | Pump state | 0 stopped, 1 running |
| 7 | 8 | Acknowledgement counter | Wrap policy to be agreed |
| 8 | 9 | Sensor A quality | 0 good, 1 uncertain, 2 bad, 3 unavailable |
| 9 | 10 | Sensor B quality | Same quality codes |
| 10 | 11 | Sensor A sample age | Seconds, saturate at 65535 |
| 11 | 12 | Sensor B sample age | Seconds, saturate at 65535 |
| 12 | 13 | Plant tick counter | Wrap policy to be agreed |

Observer endpoint: `127.0.0.1:1502`, device 1, polling every 1000 ms. All observer registers are read-only at the server boundary. Test attempted writes, including multi-register writes, to confirm refusal and unchanged plant state.

Missing data must be represented using quality and age; it must not appear as a fresh zero. Decide and document overflow, counter wrap, unavailable-value encoding, and out-of-range behavior during implementation.
