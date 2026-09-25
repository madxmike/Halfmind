# Negative Branch

## Use When

- A risk has a cause path.
- The user says "yes, but this will cause...".

## Technique

Trace the path from the action to the negative effect with if-then logic.
State the path in the doc.
Find the link with the weakest assumption.
Trim the branch: change the design, add a guard, or add a step.
State the trim next to the risk.

Check:

- The risk has a path, not a bare warning.
- The trim breaks the path before the negative effect.

## Failure Mode

The doc lists a bare risk. The author meets the risk at build time.

Source: Goldratt, "It's Not Luck", 1994 (negative branch reservation).
