# Read-back

## Use When

- A round of answers is complete.
- The agent needs confirmation of its understanding.

## Technique

Repeat the agent's understanding in the user's words.
Limit the read-back to the changed items.
Ask for a correction or a confirmation.
A confirmation is the acceptance phase of grounding.
A correction starts a repair. Repeat the repaired item.

Strong evidence of understanding:

- The user corrects one detail.
- The user adds a missing item.
- The user confirms the read-back with a reason.

Weak evidence of understanding:

- The user stays silent.
- The user agrees without a detail.

Ask for one concrete detail when the evidence is weak.

## Failure Mode

The agent continues without acceptance. Later work builds on a wrong assumption.

Source: Clark and Brennan, "Grounding in Communication", 1991.
