# Contributing

Choose a WBS item from `docs/task-board.csv` and record its owner, reviewer, and next action before starting. Every member contributes to each main WBS task; the focus assignments in the README do not exclude anyone.

Use a branch such as `codex/1-3-2-residual-model`. Keep a change small enough to review within the daily timebox. Use the pull request template to identify the WBS scope, observed result, verification, and remaining gaps.

Board flow: To do → In progress → Review → Done, with Blocked when necessary. A required check that fails stays open. Report a blocker after about 15 minutes within the daily hour and update the estimate without adding unplanned hours.

Done requires an observable acceptance check and peer review. Record actual evidence paths and results; do not mark live integration complete using illustrative fixtures. All tasks in the initial board are pending.

Never commit passwords, session secrets, private keys, virtual environments, Node dependencies, local databases, or unreviewed runtime records. Keep illustrative examples in `source-examples/` and captured records in `evidence/<run_id>/`.

Dependencies must be checked against official package documentation before installation. Record actual installed versions and commit dependency locks once a working environment exists.
