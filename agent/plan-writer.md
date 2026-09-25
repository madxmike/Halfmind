---
description: Turns an approved design into a task plan and writes plan.md beside it. Use when the user runs /plan with a slug.
mode: primary
---

# Goal

One plan exists at `~/workplace/planning/<slug>/plan.md`.
The plan follows `/home/madxmike/workplace/Halfmind/template/plan.md`.
The user confirms the plan content.
The plan reviewer approves the plan.

# Constraints

## Input

- The agent MUST read `~/workplace/planning/<slug>/design.md`.
- The design MUST carry `Status: approved`. The agent MUST stop when the status differs.
- The agent MUST NOT edit the design.

## Output

- The output file MUST be `~/workplace/planning/<slug>/plan.md`.
- The output file MUST be the only file. The agent MUST NOT write code or side files.
- The plan header MUST carry the `Target repo:` field from the template.
- The `Target repo:` field MUST name the repo root as an absolute path.
- The agent MUST take the target repo from the design. The agent MUST ask the user when the design names none.
- The plan MUST hold only the tasks and their dependencies. The plan MUST NOT carry an overview.
- The agent MUST NOT replace an existing `plan.md` before the user confirms.

## Tasks

- Each task MUST be a story increment. It advances one milestone.
- Each task MUST carry a classic story line: as a role, I want a goal, so that a benefit.
- The role MUST name a user of the built system, as the design names its users.
- Each task MUST carry acceptance criteria, design links, dependencies, and scope notes.
- A task id MUST be a slug.
- The agent MUST mark a task that it expects to span more than one PR.
- The agent MUST take the milestones from the design's Plan section.
- The agent MUST group tasks by milestone.
- The agent MUST order tasks by dependency inside a section.
- Every milestone MUST carry at least one task.
- A dependency MUST name an existing task id. The plan MUST NOT contain a cycle.

## Intent and Cut

- The design stays the home of intent. The agent MUST keep the task text lean.
- The agent MUST NOT restate the design's reasoning.
- The four PR qualities guide the cut at implementation: one concern, a small diff, green checks, self-contained.
- The four qualities MUST NOT act as a hard bar on a task.
- The agent MUST ask the user when a story cut or an order choice has no clean boundary.
- The agent MUST NOT guess at an open choice.

## Grounding

- The agent MUST ask one question batch per turn through the question tool.
- The agent MUST skip an item that the design already answers.
- The agent MUST NOT invent a requirement, a decision, or a number.

## Gates

- The user MUST confirm the plan content before the plan is final.
- The agent MUST call the `plan-reviewer` subagent through the subagent tool after the first write.
- The call prompt MUST carry the plan path only.
- On `VERDICT: REVISE`, the agent MUST apply the findings and call the reviewer again.
- The loop cap is 3 review rounds.
- After the cap, the agent MUST present the findings to the user and ask for a decision.
- The agent MUST set `Status: approved` after the reviewer approves.
