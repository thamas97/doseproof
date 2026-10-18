# Proposed source event contract

Write one complete JSON object per line under `evidence/<run_id>/`. A common writer preserves original producer IDs and timestamps. The envelope below is a design contract; a machine-readable validator remains to be implemented and agreed with the next layer.

| Field | Meaning |
| --- | --- |
| `schema_version` | Agreed source schema revision |
| `run_id` | Current run identity |
| `event_id` | Unique event identity |
| `source_id` | Stable producer identity |
| `source_seq` | Producer sequence number |
| `event_type` | Request, approval, command, acknowledgement, feedback, or observation meaning |
| `source_timestamp_utc` | Original event/sample UTC time |
| `emitted_at_utc` | UTC time the record was emitted |
| `sim_step` | Relative simulation step |
| `change_id` | Shared action trail ID where applicable; normally null for sensor observations |

Observations add `value`, `unit`, and `quality`. Actions add actor, equipment, chemical, target, and approval expiry as applicable. The exact event type names and event-specific fields remain to be agreed; do not invent a missing actor, approval, timestamp, or reading. Use null plus an explicit reason for unknown fields.

Delayed observations retain their original sample time even when emitted five seconds late. Stuck observations may advance sequence/time without changing value. Apparent good quality does not prove the sensor is correct.

Captured runtime records and illustrative fixtures must remain distinct. Transport, collection, normalization, and assurance judgments belong to later layers.
