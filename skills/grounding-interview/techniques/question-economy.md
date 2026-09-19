# Question Economy

## Use When

- The agent prepares the next question batch.
- The agent must choose between a question and an assumption.

## Technique

Ask the question with the highest information value.
One answer MUST change the doc.
Skip the question when the answer changes nothing.
Skip an item that the input already answers.
Ask one batch per turn. Keep the batch small.
Stop when the doc is writable.

## Failure Modes

- The agent stays silent on an ambiguity. Then it invents a requirement.
- The agent gives a warm acknowledgment in place of a question. Then the user believes the agent understands.
- The agent asks a question with no effect on the doc. Then the interview wastes the user's time.

Sources:

- Clark and Brennan, "Grounding in Communication", 1991 (least collaborative effort).
- Shaikh et al., "Navigating Rifts in Human-LLM Grounding", ACL 2025.
- Suri et al., "Structured Uncertainty guided Clarification for LLM Agents", ACL 2026.
