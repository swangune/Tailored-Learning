# First-Principles Learning Protocol

## Objective

Build each computational-engineering topic from its underlying physical, mathematical and algorithmic structure, while remaining aligned to the official syllabus.

## The learning stack

### Layer 0 — Diagnostic

Before teaching, test only the prerequisites needed for the active topic. Do not run a broad exam unless requested.

### Layer 1 — Phenomenon / problem

Start with the engineering or statistical problem the method is trying to solve.

Questions:

- What quantity is unknown?
- What information is given?
- What physical/statistical principle constrains the answer?
- Why is a computational method needed?

### Layer 2 — Definitions and assumptions

Define variables, dimensions/units, coordinate systems, probability assumptions, constitutive assumptions or modelling simplifications.

### Layer 3 — Mathematical derivation

Derive the governing relation at the learner's current level. Explicitly identify where each term comes from.

### Layer 4 — Minimal hand example

Use the smallest non-trivial problem that exposes the mechanism of the method.

Examples:

- FEM: one/two elements before a large mesh;
- ML: a few data points before a library call;
- simulation: a simplified control-volume/element model before multiphysics software.

### Layer 5 — Algorithm

Translate the mathematics into ordered computational steps and identify data structures, convergence criteria and numerical decisions.

### Layer 6 — Implementation

Use the language/tool expected by the module:

- MATLAB where the FEM/dynamics module expects MATLAB;
- Python where the ML outcomes explicitly require it;
- the relevant engineering simulation software for EG-M384 practical work.

### Layer 7 — Verification and interpretation

Check:

- dimensions/units;
- limiting cases;
- conservation/balance where applicable;
- numerical convergence/sensitivity;
- physical/statistical plausibility;
- model assumptions and limitations.

### Layer 8 — Retrieval / transfer

Close with an unseen question that requires reconstruction rather than recognition.

## Standard session template

1. 5–10 min prerequisite diagnostic
2. 15–25 min first-principles explanation/derivation
3. 15–25 min worked example
4. 20–40 min learner problem or implementation
5. 10 min error correction
6. 5–10 min mastery gate and log update

Times are flexible; the sequence is more important than the exact duration.
