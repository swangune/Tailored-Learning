# First-Principles Learning Protocol

## Objective

Build each computational-engineering topic from its underlying physical, mathematical and algorithmic structure while remaining aligned to the official syllabus.

This protocol is the default teaching execution method. The learner should not need to restate it when beginning a study session.

## Authoritative learning sequence

### Layer 0 — Source lock and diagnostic

Before teaching:

- resolve the scheduled module and official syllabus item;
- identify the supplied university/source material that governs the topic;
- test only the prerequisites needed for the active topic;
- open a foundation bridge only if a genuine prerequisite gap blocks progress.

### Layer 1 — Phenomenon / problem

Start with the real engineering, mathematical or statistical problem.

Establish:

- what is known;
- what is unknown;
- why the method is needed;
- what one observation, element, node, variable or physical quantity represents.

### Layer 2 — Definitions, units and assumptions

Define the important objects before using compact notation.

Include:

- feature meanings;
- units/dimensions;
- coordinate systems where relevant;
- modelling assumptions;
- probability assumptions;
- constitutive assumptions;
- index meanings.

### Layer 3 — Smallest meaningful numerical example

Use the smallest non-trivial example that exposes the mechanism.

Examples:

- FEM: one/two elements before a large mesh;
- ML: a few named observations with meaningful features before a library call;
- simulation: a simplified control-volume/element model before full software setup.

A tutor-created example must be identified as a teaching example and must not be presented as source data.

### Layer 4 — Explicit mechanics: no hidden steps

For the first complete worked example of a new mechanism:

- show every subtraction, square, sum, division, update, matrix operation, derivative or integral needed;
- derive preprocessing values instead of inserting unexplained numbers;
- explain rounding;
- keep rounded values consistent;
- do not write “similarly”, “the rest are the same”, or an equivalent shortcut when the omitted steps contain new learning.

Repeated operations may be compressed only after the learner demonstrates fluency or explicitly requests a shorter treatment.

### Layer 5 — Symbol map

After the numerical/physical mechanism is understood, map it to formal notation.

For each important symbol, establish:

- what it represents;
- its shape/dimension;
- its units where relevant;
- what its indices mean;
- which number or physical quantity in the example it corresponds to.

Prefer the notation used by the supplied reference when practical.

### Layer 6 — General equation

Present the general equation as a compressed representation of operations the learner has already seen whenever feasible.

The learner should be able to trace each term back to the concrete example.

### Layer 7 — Mathematical derivation

Derive the governing relation at the learner's current level.

The derivation must answer a concrete question, such as:

- Why is the centroid the mean?
- Why is this derivative set to zero?
- Why does this matrix term appear?
- Why does integration by parts produce this boundary term?

Identify the algebra, calculus, probability or mechanics rule used at each unfamiliar transformation.

### Layer 8 — Algorithm

Translate the mathematics into ordered computational steps.

Identify where relevant:

- initialization;
- data structures;
- assignment/update operations;
- iteration;
- stopping/convergence criteria;
- numerical decisions;
- computational cost.

Define convergence in plain language before relying on the formal criterion.

### Layer 9 — Verification and evidence

Before trusting the result, check the relevant items:

- units/dimensions;
- arithmetic consistency;
- simple or limiting cases;
- convergence;
- sensitivity;
- alternative initialization;
- physical/statistical plausibility;
- evidence for consequential modelling choices such as K, mesh density, time step or solver tolerance.

A converged algorithm is not automatically globally optimal or physically correct.

### Layer 10 — Limitations and failure modes

Explain the assumptions and situations in which the method can fail or mislead.

The learner should understand when not to trust the result.

### Layer 11 — Application / transfer

Apply the method to a meaningful engineering or machine-learning case after the mechanism is traceable.

Explicitly map the real data/physics back to the symbols and algorithm.

### Layer 12 — Retrieval and mastery gate

Close with an unseen question or task requiring reconstruction rather than recognition.

The learner should be able to reconstruct:

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

Then apply `MASTERY.md`.

## Automatic repair loop

A learner question such as:

- “why?”;
- “where did that number come from?”;
- “why this sign?”;
- “why this starting value?”;
- “does convergence mean best?”;
- “must this always be standardized?”;

is a diagnostic signal.

When it exposes an unresolved dependency:

1. stop forward progression;
2. answer the question directly;
3. return to the last secure representation;
4. rebuild the missing link;
5. resume only when the link is secure.

Conceptual questions are part of the teaching process, not interruptions to it.

## Standard session template

1. 5–10 min source/prerequisite diagnostic
2. 10–20 min concrete problem, definitions and assumptions
3. 15–30 min fully expanded numerical/physical example
4. 15–25 min symbol map, general equation and derivation
5. 15–30 min algorithm/implementation
6. 10–15 min verification, limitations and correction
7. 5–10 min transfer/mastery gate and log update

Times are flexible. Traceability and sequence are more important than exact duration.
