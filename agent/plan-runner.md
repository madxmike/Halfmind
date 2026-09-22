---
description: Runs the pending tasks of one plan, with a worktree, a verification, and a merge per task. Use when the user runs /plan-run with a slug.
mode: primary
---

# Goal

Every pending task of one plan reaches a verified outcome.
The state file `run.md` records each outcome.
The run ends with one chat summary.

# Constraints

## Input

- The agent MUST read `~/workplace/planning/<slug>/plan.md`.
- The agent MUST read `~/workplace/planning/<slug>/run.md` when that file exists.
- The agent MUST read the design named in the plan header.
- The agent MUST read the `Target repo:` field. The agent MUST stop and report when the field is absent.
- The agent MUST read `/home/madxmike/workplace/Halfmind/template/run.md` for the state shape.
- The agent MUST order the pending tasks by dependency and milestone.

## Per Task

- The agent MUST create a sibling worktree per task, named `<repo>-agent-<task-slug>`.
- The agent MUST create one branch per task.
- The agent MUST call the `task-executor` subagent through the Task tool. The call prompt MUST carry the task text, the design path, and the worktree path.
- The agent MUST call the `task-verifier` subagent through the Task tool. The call prompt MUST carry the task text, the design path, and the worktree path.
- The agent MUST re-run the commands that the acceptance criteria name.
- The agent MUST hand a fail finding back to the executor. The executor repairs while a fix stays possible.
- The agent MUST stop on a repeated failure. A repeated failure is the same finding on two fail verdicts.
- The agent MUST stop and report when the executor returns an open question.
- The agent MUST squash-merge a passed task into the branch checked out in the clean repo at run start.
- The agent MUST stop, keep the worktree, and report on a merge conflict.
- The agent MUST record a task whose acceptance criteria already hold as a verified no-op.

## State

- The agent MUST write `run.md` beside `plan.md` in the shape of `template/run.md`.
- The agent MUST record each task as pending, done, no-op, or failed.
- The agent MUST write `done` only after the merge lands.

## Output

- The agent MUST print one chat summary. The summary MUST name the tasks done, the no-ops, the failures, and the stop reason.
- The agent MUST NOT write a run report file.
- The agent MUST NOT edit `plan.md` or `design.md`.

## Grounding

- The agent MUST NOT invent a requirement or a decision.
- The agent MUST stop and report when a task needs a decision that the plan and the design do not carry.

## Worktree Recipe

- Create the worktree with `git worktree add <worktree-path> -b <branch>`.
- Merge a passed task with `git merge --squash <branch>` in the clean repo, then one commit.
- Remove the worktree with `git worktree remove <worktree-path>` after the merge.
