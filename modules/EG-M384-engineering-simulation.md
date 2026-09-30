# EG-M384 — Engineering Simulation

## Official syllabus

1. Review computational modelling and simulation in design
2. Develop modelling strategies from problem definition, assumptions, simplifications and governing physics
3. CAD/meshing considerations and effects on accuracy
4. Finite volume and finite element analysis techniques: advantages, disadvantages and common applications
5. Tools for flow, heat transfer, structural mechanics and multiphysics interaction
6. Optimisation tools
7. Case studies: performance investigation and design/implementation optimisation

## Foundation bridge — tailored, not additional syllabus

Open only when required:

- units/dimensional consistency;
- conservation principles (mass, momentum, energy) at conceptual level;
- boundary and initial conditions;
- discretisation intuition;
- linear algebra/numerical solution intuition;
- distinction between model error, discretisation error and solver/convergence issues.

## First-principles path

### 1. Why computational modelling exists

Physical system -> engineering question -> model -> numerical approximation -> result -> engineering decision.

### 2. Modelling strategy

Define quantity of interest -> governing physics -> domain -> assumptions -> simplifications -> boundary/initial conditions -> fidelity vs cost.

### 3. CAD and mesh

Geometry idealisation -> mesh topology/size/quality -> local gradients -> independence/convergence reasoning -> impact on accuracy and computational cost.

### 4. FVM vs FEM

Start from what each method approximates and how the governing equations are discretised; then compare typical strengths, weaknesses and applications without treating either as universally superior.

### 5. Single-physics to multiphysics

Understand each physics separately before coupling. Identify transferred quantities and coupling direction/strength.

### 6. Optimisation

Design variables -> objective -> constraints -> simulation as evaluator -> search/optimisation loop -> sensitivity to modelling error.

### 7. Case studies

For each case: engineering question -> assumptions -> model -> mesh -> solve -> verification checks -> interpret -> modify design -> compare against operational criteria -> report.

## Report-oriented evidence

Because the supplied assessment is report-based, each practical topic should eventually produce concise evidence of:

- modelling rationale;
- assumptions;
- mesh/numerical choices;
- result interpretation;
- limitations;
- design decision or optimisation rationale.
