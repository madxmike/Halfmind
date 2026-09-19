# TOC Measures

## Use When

- The agent sets goals and measures.
- The user asks "how do we know it worked?"

## Technique

Name the measure for each goal:

- Throughput: the rate the system delivers value.
- Inventory: the work in process, not delivered.
- Operating expense: the cost to run the system.

A goal SHOULD move one measure without damage to the others.
Find the constraint: the step that limits the throughput.
A change at the constraint moves the system. A change elsewhere moves little.

Check:

- Each goal names one measure.
- The design addresses the constraint, not a symptom.

## Failure Mode

The goal names no measure. The team cannot tell success from failure.

Source: Goldratt, "The Goal", 1984 (throughput, inventory, operating expense).
