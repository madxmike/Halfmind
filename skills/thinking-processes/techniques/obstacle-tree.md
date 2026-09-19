# Obstacle Tree

## Use When

- An obstacle blocks the plan.
- The user says "we cannot do it without...".

## Technique

List each obstacle as a fact: "we cannot do X because Y".
Pair each obstacle with one intermediate objective.
The intermediate objective MUST remove the obstacle.
Order the intermediate objectives by dependency.
Mark an obstacle with no objective as open. Ask the user for a decision.

Check:

- Each obstacle has one intermediate objective.
- The order respects the dependencies.

## Failure Mode

The plan hides an obstacle. The work stops at the obstacle.

Source: Goldratt, "It's Not Luck", 1994 (prerequisite tree).
