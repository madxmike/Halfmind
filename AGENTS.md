# Halfmind

Halfmind is the global opencode configuration. It contains agents, commands,
skills, and plugins. Symbolic links connect `~/.config/opencode/` to this
repository. All text in this repository MUST follow the rules in this
document.

## The Model

An agent is a small operation. It is almost a function. It receives an input
and it returns an output. The operation uses judgment, so the agent is a
**fuzzy program**. The output is not exact.

Agents follow the Unix model. An agent does one thing. The name of the agent
MUST state that one thing. A caller MUST receive one result. Each call MUST
NOT depend on a previous call. The input MUST carry all necessary data. A
subagent MUST get a fresh context.

### Composition

Operations chain together. Text flows from one operation to the next. The
output of one operation is the input of the next operation.

| Element | Unix equivalent | Behavior |
| --- | --- | --- |
| Primary agent | Shell | talks to the user, starts operations |
| Subagent | Function | does one fuzzy operation |
| Task tool | Pipe | sends one input, receives one output |
| Command | Shell script | runs a fixed chain of operations |
| Skill | Manual page | loads on demand |
| Plugin | System call | runs exact code, no judgment |
| opencode.jsonc | Shell profile | declares the elements |

A command MAY chain agents and tools. A skill MUST load only when needed. A
plugin MUST contain exact code and no judgment.

## Language Rules

All text in this repository MUST comply with two standards. The standards
are ASD-STE100 Simplified Technical English and RFC 2119.

### RFC 2119 Keywords

This document uses the key words of RFC 2119. The key words are MUST, MUST
NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and
OPTIONAL. These words carry the definitions of RFC 2119. Only the uppercase
form carries these definitions, as RFC 8174 states. A lowercase form has the
usual English meaning.

### STE100 Rules

- A sentence in a procedure MUST contain no more than 20 words.
- A descriptive sentence MUST contain no more than 25 words.
- A sentence MUST contain one idea. A paragraph MUST cover one topic.
- Use the active voice. Use the passive voice only when the actor is unknown.
- Use simple tenses only.
- A verb MUST NOT use the present perfect or the `-ing` form.
- Contractions and semicolons MUST NOT occur.
- One word MUST have one meaning. Use the same term for the same concept.
- Idioms, slang, and metaphors MUST NOT occur, unless the glossary defines
  the term as a technical name.
- A series of three or more items MUST use a vertical list.

### Goals and Constraints

A capability definition MUST state a goal and constraints. A capability
definition MUST NOT state a sequence of steps.

- A goal states the wanted result.
- A constraint states a condition on the result or the method.

Steps are REQUIRED only when exactness or safety needs them.

Good example:

> Goal: produce a peanut butter and jelly sandwich.
> Constraint: the sandwich MUST use JIF peanut butter.

Bad example:

> Step 1: get bread. Step 2: spread peanut butter. Step 3: add jelly.

The good example lets the agent choose a method. The bad example fixes the
method. A fixed method wastes tokens and breaks when the environment
changes.

## Capability Files

| Kind | Path | Frontmatter | Trigger |
| --- | --- | --- | --- |
| Agent | `agent/<name>.md` | `description`, `mode`, `model`, `permission` | The `description` field. |
| Command | `command/<name>.md` | `description`, `agent`, `model` | The user types `/name`. |
| Skill | `skills/<name>/SKILL.md` | `name`, `description` | The `description` field. |
| Plugin | `plugin/<name>.ts` | None | A hook or a tool call. |

Rules:

- A file name MUST use lowercase letters and hyphens.
- A skill folder name MUST equal the `name` field.
- An agent `description` MUST state the operation and the trigger.
- An agent with `mode: subagent` MUST do one small operation.
- A command input MUST come from the `$ARGUMENTS` string.
- A capability file MUST state a goal and constraints. It MUST NOT state a
  sequence of steps.
- A capability file SHOULD be short. A short file costs fewer tokens.
- For exact field shapes, load the `customize-opencode` skill.

## Operations

- opencode reads the configuration one time at startup.
- A restart is REQUIRED after a configuration change.
- JSON configuration files MUST declare the `$schema` key.
- `opencode.jsonc` MUST validate against the schema at
  `https://opencode.ai/config.json`.
- Skills MUST register through `skills.paths` in `opencode.jsonc`.
- Symbolic links map the configuration folders into `~/.config/opencode/`.
- A commit MUST NOT contain a secret.

## Glossary

- **Agent**: a small fuzzy operation. It is almost a function.
- **Command**: a fixed chain of operations. The user starts it with `/name`.
- **Constraint**: a condition on a result or a method.
- **Fuzzy program**: an operation with judgment inside. Its output is not
  exact.
- **Goal**: the wanted result of an operation.
- **Operation**: one unit of work with an input and an output.
- **Pipe**: a connection between two operations.
- **Plugin**: exact code with no judgment. A system call in the model.
- **Primary agent**: the agent that talks to the user. It starts operations.
- **Skill**: a manual page. It loads on demand.
- **Subagent**: an operation that another agent starts. It has a fresh
  context.
- **Task tool**: the pipe for a subagent call.
- **Unix model**: the rule that each operation does one thing, and
  operations chain together.
