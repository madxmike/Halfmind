# Run: <Title>

- Plan: <absolute path to plan.md>
- Target repo: <absolute path to the repo root>
- Started: <YYYY-MM-DD>
- Updated: <YYYY-MM-DD>

This file sits beside `plan.md`. It records the status of each plan task.

| Task | Status | Commit | Verdict |
| --- | --- | --- | --- |
| <task-slug> | pending | none | none |

Status values:

- `pending`: the task is not started.
- `done`: the task passed, and the merge landed.
- `no-op`: the task passed with no change. Its acceptance criteria already hold.
- `failed`: the task stopped the run. The verdict carries the finding.
