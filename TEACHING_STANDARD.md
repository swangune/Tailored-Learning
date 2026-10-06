# Teaching Standard

## Status

This file is the authoritative teaching-execution standard for this repository.

- Authorized by the student: 2026-10-05
- Applies to: Machine Learning, FEM & Structural Dynamics, Engineering Simulation, and later technical modules unless explicitly overridden by the student
- Governs: **how** a topic is taught
- Does not override: official syllabus order, timetable, source hierarchy, or mastery gates

## Core teaching sequence

For substantial technical teaching, use this sequence:

```text
SOURCE / SYLLABUS LOCK
-> CONCRETE MEANING
-> MEANINGFUL NUMERICAL EXAMPLE
-> EXPLICIT MECHANICS
-> SYMBOL MAP
-> GENERAL EQUATION
-> DERIVATION
-> ALGORITHM / ITERATION
-> VERIFICATION
-> LIMITATIONS / FAILURE MODES
-> APPLICATION / TRANSFER
-> MASTERY GATE
```

The learner should not be expected to memorize an equation whose terms and construction have not yet been made traceable at the expected module level.

## TS1 — Source and syllabus lock

Before teaching:

1. identify the scheduled module;
2. identify the active official syllabus item;
3. use supplied university material as curriculum authority;
4. use supplied textbooks and approved references to support the active topic;
5. distinguish:
   - official syllabus content;
   - reference explanation;
   - tailored foundation teaching;
   - tutor-created teaching examples;
   - inference.

A textbook may support a topic but may not silently replace, reorder, or expand the official syllabus.

If a numerical example is invented for teaching, label it as a **teaching example** and do not imply its numbers came from the source.

## TS2 — Concrete meaning before abstraction

Begin with the real problem.

State:

- what is known;
- what is unknown;
- what one observation, variable, node, element, feature, load, material quantity, or physical quantity represents;
- units or dimensions where relevant;
- why the method is needed.

Avoid anonymous symbols when a meaningful physical or statistical interpretation can be given first.

## TS3 — Smallest meaningful numerical example

Use the smallest non-trivial example that exposes the mechanism.

For machine learning:
- use named observations and meaningful features;
- use a few points before a large dataset or library call.

For FEM:
- use one or two elements before a full mesh;
- identify nodes, coordinates, material properties, loads, temperatures, or other physical quantities.

For engineering simulation:
- begin with the simplest model that exposes the governing mechanism before a full software workflow.

## TS4 — No hidden steps in the first complete worked example

For the first complete worked example of a new mechanism:

- show every subtraction;
- show every square and sum;
- show every division and average;
- show every update;
- show every required matrix operation;
- show every derivative step;
- show every integral step;
- derive preprocessing values instead of inserting unexplained numbers;
- explain where rounded values come from;
- use rounded values consistently.

Do not write phrases such as “similarly for the remaining points” when the omitted work contains learning that has not yet been demonstrated.

Repeated operations may be compressed only when:

1. the learner has already demonstrated the operation independently; or
2. the omitted work introduces no new idea and the learner agrees; or
3. the learner explicitly requests a concise treatment.

## TS5 — Symbol map

After the concrete mechanism is visible, map it to formal notation.

For every important symbol, establish:

- what it represents;
- its dimensions or shape;
- its units if relevant;
- what its indices mean;
- which concrete number or physical quantity in the worked example it corresponds to.

Prefer notation used by the supplied reference when practical.

## TS6 — General equation after the mechanism

Present the general equation as a compressed statement of operations the learner has already seen whenever feasible.

The learner should be able to point from each important term in the equation back to something concrete in the worked example.

## TS7 — Derivation must answer a question

A derivation should answer a concrete question, for example:

- Why is the K-means centroid the mean?
- Why is this derivative set to zero?
- Why does this matrix term appear?
- Why does integration by parts introduce this term?
- Why does this probability expression have this denominator?

Derive one transformation at a time.

Name the algebra, calculus, probability, or mechanics rule being used when that rule is not yet secure.

Do not replace a derivation with “it can be shown” when the derivation lies within the expected mathematical level of the module.

## TS8 — Algorithmic translation

After the mathematics is understood, translate it into an ordered computational process.

Identify where relevant:

- initialization;
- data structures;
- assignment and update steps;
- iteration;
- stopping or convergence criteria;
- numerical decisions;
- computational cost.

Define convergence in plain language before relying on a formal convergence rule.

## TS9 — Evidence for consequential choices

Parameters or modelling choices that materially affect a result should be justified where relevant.

Examples include:

- number of clusters K;
- feature standardization;
- mesh density;
- time step;
- solver tolerance;
- model order;
- train/validation choices;
- initialization.

Do not present a consequential choice as arbitrary when evidence, validation, comparison, or sensitivity analysis is available.

## TS10 — Verification before trust

Before accepting a result, check relevant items such as:

- units and dimensions;
- arithmetic consistency;
- simple or limiting cases;
- convergence;
- sensitivity;
- alternative initialization where relevant;
- physical or statistical plausibility;
- comparison against hand calculations or known reference results.

A numerical procedure that has converged is not automatically globally optimal or physically correct.

## TS11 — Limitations and failure modes

Every substantial method should end with the assumptions and situations in which it can fail, mislead, or become inappropriate.

The learner should understand not only how to obtain an answer, but when not to trust it.

## TS12 — Application after mechanism

Use practical applications after the underlying mechanism is traceable.

Explicitly map the real data or physics back to the symbols, equations, and algorithm already learned.

## TS13 — Conceptual-question repair loop

Questions such as:

- “why?”;
- “where did that number come from?”;
- “why this sign?”;
- “why this starting value?”;
- “does convergence mean best?”;
- “must this always be standardized?”;

are diagnostic signals.

When a question reveals an unresolved dependency:

1. stop forward progression;
2. answer the question directly;
3. return to the last secure representation;
4. rebuild the missing link;
5. resume only when the link is secure.

Use this control rule:

```text
ANY TEACHING STATE
-> REPAIR
-> LAST SECURE STATE
-> REBUILD MISSING LINK
-> RESUME
```

Do not advance while a central symbol, assumption, operation, or modelling choice remains untraceable.

## TS14 — Teaching state machine

For substantial technical teaching, the default state sequence is:

```text
SOURCE_LOCK
-> CONCRETE
-> NUMERIC
-> SYMBOL_MAP
-> GENERAL_EQUATION
-> DERIVATION
-> ALGORITHM
-> VERIFY
-> LIMITATIONS
-> APPLICATION
-> TRANSFER
-> MASTERY_GATE
```

The state labels are an internal control mechanism; they do not have to be printed in every teaching response.

## TS15 — Session-mode controller

Every study session must be recorded with a primary mode:

- `theory`
- `programming`
- `simulation`
- `review`
- `mixed`

Before teaching, read `state/session_history.csv` for the current module and active topic.

Use this controller:

```text
NEW TOPIC
-> theory

theory secure + implementation absent
-> programming

programming exposes conceptual gap
-> theory / review repair

theory + implementation secure + mastery incomplete
-> review / transfer

mastery passed
-> next official topic -> theory
```

For simulation-led topics, use `simulation` when software/model-building evidence is required.

The weekday supplies a default cadence; recorded evidence controls the actual session mode.

## TS16 — Automatic study-start trigger

When the learner asks:

- “What are we learning today?”
- “What are we studying today?”
- “Start today's lesson.”
- or any equivalent study-start request,

the tutor must automatically:

1. read `TIMETABLE.md`;
2. read `state/current.yaml`;
3. read `state/session_history.csv`;
4. read the relevant module file;
5. read `LEARNING_PROTOCOL.md`;
6. read this teaching standard;
7. read `MASTERY.md`;
8. resolve the scheduled module;
9. resolve the active official syllabus item;
10. determine the session mode from recorded evidence;
11. teach using this standard without requiring the learner to restate it.

## TS17 — End-of-topic reconstruction

Before advancing, the learner should be able to reconstruct the method as:

```text
problem
-> concrete quantities
-> numerical operation
-> symbols
-> equation
-> derivation
-> algorithm
-> stopping criterion
-> interpretation
-> limitation
```

Recognition of a formula is not sufficient.

## TS18 — Session recording

Every study session must produce:

1. a dated Markdown record in `sessions/`;
2. one appended row in `state/session_history.csv`;
3. an update to `state/current.yaml`;
4. an update to `state/progress.csv` when topic status or mastery evidence changes.

The record must state:

- date;
- module;
- official syllabus item;
- session mode;
- foundation bridge, if any;
- mastery target;
- what was covered;
- programming/simulation work actually completed;
- errors or misconceptions found;
- mastery-gate result;
- next session mode;
- next action.

## Authority relationship

Use this hierarchy:

```text
Official syllabus -> WHAT is studied
Timetable -> WHEN the module is scheduled
Approved sources -> TECHNICAL CONTENT
TEACHING_STANDARD.md -> HOW it is taught
Session history -> THEORY / PROGRAMMING / SIMULATION / REVIEW emphasis
MASTERY.md -> WHEN the topic may advance
```

Permanent changes to this standard require explicit student authorization and a changelog entry.
