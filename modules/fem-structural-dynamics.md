# Finite Element Method & Structural Dynamics

## Official prerequisites from the supplied syllabus

- basic finite elements and dynamics;
- basic MATLAB programming.

## Foundation bridge — tailored prerequisite recovery

This bridge exists because the official syllabus assumes prior knowledge. It is not a replacement syllabus.

Open only where diagnostics show a gap:

1. matrix/vector algebra used in assembled systems;
2. derivatives, integrals and basic ODE/PDE interpretation;
3. mechanics essentials: equilibrium, stress, strain, constitutive relation;
4. 1D bar/truss FEM: interpolation, strain-displacement relation, element stiffness, assembly and boundary conditions;
5. dynamics essentials: mass, stiffness, damping, free/forced vibration, SDOF equation;
6. MATLAB: vectors/matrices, functions, loops only where needed, plotting and linear solves.

Return target: the first blocked official topic.

## Official Part 1 — Finite element analysis

### 1. Isoparametric finite elements

First-principles route: physical coordinates -> natural coordinates -> interpolation -> Jacobian -> derivative transformation -> element formulation.

### 2. Numerical integration

Exact integration need -> quadrature concept -> Gauss points/weights -> polynomial exactness -> element integration -> error implications.

### 3. 2D heat transfer

Conservation of energy -> Fourier conduction -> governing PDE -> weak/finite-element form -> conductivity matrix/source terms -> boundary conditions.

### 4. Quadrilateral and high-order elements

Interpolation order -> shape functions -> mapping -> numerical integration -> accuracy/cost trade-off and element distortion.

### 5. Mesh generation

Geometry discretisation -> element quality -> refinement -> convergence -> error sensitivity.

### 6. 2D and 3D elasticity

Equilibrium -> kinematics -> constitutive law -> weak form -> B/D matrices -> element stiffness -> stress recovery.

### 7. Introduction to non-linear problems

Identify source of nonlinearity -> incremental/iterative solution idea -> residual/equilibrium -> convergence concept. Do not over-expand beyond the introductory syllabus scope unless instructed.

## Official Part 2 — Dynamic analysis

### 8. Time integration for transient heat transfer

Semi-discrete system -> time derivative -> step-by-step integration -> stability/accuracy.

### 9. Explicit and implicit time integration

Update dependence -> computational cost -> stability constraints -> accuracy -> suitable problem classes.

### 10. Fundamentals of structural dynamics

Review SDOF -> MDOF -> modal analysis from equation of motion and eigenproblem.

### 11. Structural dynamics

Mass/stiffness/damping systems -> forcing -> resonance -> modal response and physical interpretation.

### 12. Newmark method

Equation of motion -> assumed acceleration/velocity update relations -> effective system -> time marching -> stability/parameter effects.

## Implementation policy

MATLAB is the default implementation language because the supplied module notes state that MATLAB programming is used throughout.
