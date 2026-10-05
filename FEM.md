Steady-State 2D Heat Flow and FEM from First Principles

Teach me steady-state two-dimensional heat conduction and derive its finite-element formulation from first principles.

Assume I am comfortable with basic algebra and matrices, but I have historically learned mathematics by memorising formulas. I am not yet comfortable with symbolic differentiation, integration, limits, partial derivatives, gradients, divergence, coordinate transformations, Jacobians, or derivations involving functions rather than numbers.

Your objective is not merely to show me the final FEM equations.

Your objective is to make every mathematical and physical step traceable so that I can reconstruct the derivation myself.


## SOURCE-LOCKED TEACHING CONTRACT

This document is governed by a strict source rule.

### 1. Supplied textbooks are the lesson-content authority

All substantive lesson content must be supported by the textbooks supplied with this project.

The tutor is not permitted to manufacture, import from memory, or silently add:

- equations;
- physical laws;
- mathematical theorems;
- definitions;
- terminology;
- symbols;
- sign conventions;
- coordinate conventions;
- FEM conventions;
- assumptions;
- examples;
- worked problems;
- derivations;
- interpretation claims;
- special cases;
- generalisations;
- shortcuts;
- rules of thumb.

General model knowledge is not an authorised source for lesson content.

If the supplied textbooks do not support a statement needed for the lesson, stop and state that the supplied sources do not currently establish it. Do not fill the gap from memory.

### 2. Explanation may simplify; content may not be invented

The tutor may make textbook material easier to understand by:

- breaking a textbook derivation into smaller steps;
- expanding algebraic steps that the textbook compresses;
- explaining one symbol at a time;
- restating a textbook definition in simpler language while preserving its meaning;
- connecting two consecutive textbook steps explicitly;
- describing the physical intuition already supported by the textbook;
- checking dimensions, matrix sizes, or algebra when those checks follow directly from the source material.

These actions are explanatory decompositions of supplied material, not permission to introduce new theory.

A helpful explanation must remain traceable to the supplied source.

### 3. Source declaration is mandatory

At the beginning of each lesson or milestone, declare:

- **Primary textbook:**
- **Chapter / section / page(s), when available:**
- **Supporting textbook(s), if used:**
- **Notation convention being followed:**
- **Any source conflict or alternative convention:**

Do not blend multiple textbooks into a synthetic convention.

If two supplied textbooks use different notation or conventions:

1. show each convention separately;
2. identify its source;
3. explain only the relationship that can be established from the supplied sources;
4. declare which source convention will govern the current derivation;
5. use that convention consistently.

### 4. Every technical term must be sourced, named, and defined before use

No technical term may appear as unexplained vocabulary.

Before first use, give:

1. the term used by the supplied textbook;
2. its plain-language explanation;
3. its technical meaning in that source;
4. its physical meaning where the source provides one;
5. its mathematical role;
6. units where applicable;
7. any source-approved synonym or alternative terminology.

This applies to terms such as domain, boundary, field, flux, gradient, divergence, Laplacian, residual, weighting function, test function, trial function, strong form, weak form, Galerkin method, shape function, natural coordinate, isoparametric mapping, Jacobian, determinant, inverse, B-matrix, conductivity matrix, element matrix, assembly, essential boundary condition, and natural boundary condition.

If the textbook has not yet defined a term, do not use the term as if it were already understood.

### 5. Every symbol must be introduced before it appears in a derivation

At first use, state:

- the symbol exactly as used by the governing textbook;
- how it is read or pronounced where useful;
- what it represents;
- whether it is a scalar, vector, matrix, function, coordinate, operator, region, boundary, or other object;
- whether it is known or unknown;
- whether it is constant or variable in the current context;
- what variables it depends on;
- its units where applicable;
- its dimensions or matrix size where applicable.

Do not silently rename textbook symbols for convenience.

Maintain a running symbol ledger during the lesson.

### 6. Classify every important statement

Before using an important relation, identify from the supplied source whether it is being used as a:

- physical conservation principle;
- constitutive law or material relation;
- mathematical definition;
- mathematical identity;
- mathematical theorem;
- modelling assumption;
- coordinate convention;
- sign convention;
- interpolation assumption;
- numerical approximation;
- numerical method.

Do not call every equation a law.

### 7. Absolute no-magic rule

No equation may appear without an explained origin.

No equation may transform into another equation through an unexplained jump.

For every meaningful transition, state:

- what changed;
- why it changed;
- the exact mathematical operation used;
- why that operation is valid here;
- which source relation, rule, or definition authorises it;
- which symbols are variables;
- which are constants;
- what is being held fixed;
- what assumptions are active;
- what the resulting expression means physically and mathematically.

Do not hide several operations inside phrases such as:

- "by definition";
- "obviously";
- "it follows that";
- "similarly";
- "using the usual result";
- "after simplification";
- "we know that";
- "applying integration by parts";
- "using the divergence theorem";
- "using the Jacobian";
- "transforming the derivatives";
- "using the standard B-matrix";
- "combining the equations".

If a supplied textbook compresses several steps, expand those steps without changing the source result.

### 8. Scalar meaning before compact notation

Do not use compact vector, tensor, operator, or matrix notation before the component meaning is established from the supplied source.

For example, do not introduce a compact gradient, divergence, Laplacian, Jacobian transformation, B-matrix, or matrix Fourier relation before the learner has seen and understood the component equations that the notation collects.

Matrices organise previously understood scalar relations; they must not conceal unexplained mathematics.

### 9. Milestones are mandatory and must state why they exist

Every major conceptual stage must be explicitly declared as a milestone.

Use this structure:

#### Milestone N — [textbook-supported name]

**Source:** identify the supplied textbook and section.

**Why this milestone exists:** explain why this stage is necessary and what later step depends on it.

**What we already know:** list only previously established, source-supported facts.

**New terminology:** define all new textbook terms.

**New symbols:** introduce all new symbols before using them.

**Statement type:** identify whether the central relation is a law, theorem, definition, assumption, approximation, or method.

**Target:** state what the milestone is trying to establish without presenting an unexplained final formula.

**Derivation:** proceed one justified operation at a time.

**Milestone achieved:** state exactly what has now been established.

**Why the next milestone is now possible:** explain the logical dependency.

A milestone is not complete because a formula has been displayed. It is complete only when the learner can reconstruct where that formula came from.

### 10. Mandatory major milestones for this heat-conduction FEM pathway

The lesson must explicitly declare the major transitions already required by this document, including:

1. mathematical prerequisites established;
2. thermal quantities and notation established;
3. conservation of thermal energy established;
4. Fourier's law established;
5. governing differential equation derived;
6. strong form declared and explained;
7. boundary conditions classified;
8. residual defined;
9. weighted-residual statement constructed;
10. integration-by-parts / equivalent textbook operation established;
11. weak form derived;
12. Galerkin formulation established;
13. element interpolation derived;
14. natural-coordinate description established;
15. coordinate mapping established;
16. Jacobian derived;
17. derivative transformation derived;
18. shape-function derivative / B-matrix established;
19. conductivity matrix established;
20. element conduction matrix derived;
21. source and boundary vectors derived;
22. element integration completed;
23. global assembly completed;
24. essential boundary conditions applied;
25. nodal temperatures solved;
26. temperature gradients recovered;
27. heat fluxes recovered and interpreted.

Do not skip a milestone because the final equation is familiar.

### 11. No manufactured examples

Worked examples must come from the supplied textbooks unless the user explicitly authorises a tutor-created example.

If a textbook example is too large, the tutor may explain only a smaller portion of that same example, but may not change the data or invent replacement values without permission.

### 12. Source-gap rule

When a required definition, convention, or derivation cannot be located in the supplied textbooks:

1. say exactly what is missing;
2. identify why it blocks the current milestone;
3. do not manufacture a replacement;
4. ask the learner whether another supplied source should be searched or whether outside material is explicitly authorised.

The default is always source fidelity over completion speed.

The final destination is to derive, in logical order:

1. the mathematical foundations needed for the derivation;
2. conservation of thermal energy;
3. Fourier's law;
4. the governing differential equation;
5. the strong form;
6. boundary conditions;
7. the residual;
8. the weighted-residual statement;
9. the weak form;
10. the Galerkin formulation;
11. the three-node linear triangular element;
12. local or natural coordinates;
13. coordinate mapping;
14. the Jacobian matrix;
15. the determinant and inverse of the Jacobian;
16. transformation of derivatives from local coordinates to global coordinates;
17. the shape-function derivative matrix;
18. the B-matrix;
19. the conductivity matrix D;
20. the element conduction matrix K^(e);
21. the internal heat-generation vector;
22. the prescribed heat-flux boundary vector;
23. numerical or analytical integration over the element;
24. assembly into the global system;
25. application of essential boundary conditions;
26. solution for nodal temperatures;
27. recovery of temperature gradients and heat fluxes.

Do not begin with the heat equation or a finite-element matrix.

Start with the mathematics required to understand every later step.

## Fundamental teaching rule: no unexplained jumps

Do not write phrases such as:

- "by definition";
- "obviously";
- "it follows that";
- "similarly";
- "using the usual result";
- "after simplification";
- "we know that";
- "taking the derivative";
- "applying integration by parts";
- "using the divergence theorem";
- "using the Jacobian";
- "transforming the derivatives";
- "from the standard B-matrix";

unless you immediately show and explain the exact operation being used.

If one displayed equation changes into another, show every meaningful algebraic or calculus step between them.

Do not compress several mathematical operations into one line.

For each important transition, answer:

- What changed?
- Why did it change?
- What mathematical rule was used?
- Why is that rule valid here?
- Which symbols are variables?
- Which symbols are constants?
- What is being held fixed?
- What assumptions are required?
- What are the dimensions of the vectors and matrices involved?
- What physical quantity does the result represent?

## Start with the derivative itself

Before using any derivative in the heat-flow derivation, teach and derive:

- what a variable is;
- what a constant is;
- what a function is;
- independent and dependent variables;
- finite change;
- the meaning of Δx;
- the difference between a value and a change in a value;
- average rate of change;
- the difference quotient;
- a secant line;
- why a derivative is required;
- what a limit means intuitively;
- why Δx → 0 does not mean substituting Δx = 0;
- the derivative from the limit definition;
- tangent slope;
- the distinction between Δx, dx, d/dx, and a derivative.

Use the symbols, examples, and numerical values supplied by the governing textbook. Do not invent a numerical example unless the learner explicitly authorises one.

For example, derive the derivative of

$$
f(x)=ax+b
$$

directly from

$$
\frac{df}{dx}
=
\lim_{\Delta x\to0}
\frac{f(x+\Delta x)-f(x)}{\Delta x}.
$$

Do not use a memorised differentiation rule to prove the result.

Then introduce functions of two variables:

$$
T=T(x,y).
$$

Derive from finite differences and limits:

$$
\frac{\partial T}{\partial x}
$$

and

$$
\frac{\partial T}{\partial y}.
$$

Explicitly explain what it means to hold y fixed and vary x, and vice versa.

Then derive and explain second partial derivatives before they appear in the heat equation.

## Build vector calculus gradually

Do not introduce compact notation such as

$$
\nabla,\qquad
\nabla T,\qquad
\nabla\cdot\mathbf q,\qquad
\nabla^2T
$$

until the corresponding component equations are understood.

For every vector-calculus concept, define:

- its full name;
- why it has that name;
- its component form;
- its geometrical meaning;
- its physical meaning;
- its units where useful;
- how it connects to earlier mathematics.

Do this separately for:

- vector;
- dot product;
- unit vector;
- normal vector;
- gradient;
- normal derivative;
- flux;
- normal flux;
- divergence;
- Laplacian.

## Derive conservation from a small region

Use a small rectangular control region with dimensions

$$
\Delta x,\qquad \Delta y
$$

and physical thickness

$$
t.
$$

Define "control volume" before using the term.

Use an ASCII diagram.

Identify all faces.

For each heat-transfer term, show:

- the relevant heat-flux component;
- the face area;
- why flux multiplied by area gives heat-transfer rate;
- whether the contribution enters or leaves;
- the sign convention.

Write the complete finite energy balance before cancelling, dividing, or taking a limit.

When an expression such as

$$
q_x(x+\Delta x,y)-q_x(x,y)
$$

appears, first identify it as a finite difference.

Then divide by Δx.

Then explicitly take the limit

$$
\Delta x\to0.
$$

Only after this identify the result as

$$
\frac{\partial q_x}{\partial x}.
$$

Perform the corresponding derivation separately for the y-direction.

Do not replace a finite difference quotient with a derivative without showing the limit.

## State all assumptions

Create an "Assumptions used so far" subsection whenever a new assumption becomes necessary.

Possible assumptions include:

- continuum assumption;
- steady-state condition;
- two-dimensional approximation;
- negligible temperature variation through the thickness;
- sufficiently smooth fields;
- constant or variable conductivity;
- isotropic or anisotropic material;
- constant or variable thickness;
- presence or absence of internal heat generation;
- no phase change;
- no thermal contact resistance unless introduced;
- sign convention for heat flux;
- small-control-volume limiting process.

Do not silently assume any of them.

If an equation simplifies because of an assumption, show:

1. the general equation;
2. the assumption;
3. the resulting specialised equation.

## Derive Fourier's law carefully

Define:

- heat;
- heat-transfer rate;
- heat flux;
- thermal conductivity;
- temperature gradient.

Start in one dimension:

$$
q_x=-k_x\frac{\partial T}{\partial x}.
$$

Explain the negative sign physically and algebraically.

Then extend to two dimensions:

$$
q_x=-k_x\frac{\partial T}{\partial x},
$$

$$
q_y=-k_y\frac{\partial T}{\partial y}.
$$

Only after the scalar forms are understood, introduce

$$
\mathbf q=
\begin{bmatrix}
q_x\\
q_y
\end{bmatrix}
$$

and the conductivity matrix

$$
[D].
$$

Explain why anisotropic conductivity requires a matrix.

## Strong form

Derive the governing equation from conservation plus Fourier's law.

Do not simply present it.

Then explain exactly why it is called the strong form.

Define Ω as the domain and Γ as its boundary.

Introduce and explain:

- prescribed-temperature boundary;
- prescribed-flux boundary;
- Dirichlet condition;
- Neumann condition;
- essential condition;
- natural condition.

Use one heat-flux sign convention and retain it throughout the lesson.

Show explicitly which derivative order appears in the strong form.

## Teach integration before using the weak form

Before using integration by parts, teach:

- accumulation;
- indefinite integral;
- definite integral;
- one-dimensional integration;
- area integration;
- double integrals;
- meaning of dx;
- meaning of dy;
- meaning of dA;
- boundary integration;
- meaning of ds;
- product rule;
- derivation of integration by parts from the product rule.

First derive the one-dimensional identity.

Only afterwards extend the idea to the multidimensional form needed for FEM.

Before using the divergence theorem, explain:

- what divergence means;
- what an outward unit normal means;
- what a normal flux means;
- what a domain integral means;
- what a boundary integral means.

Explain the divergence theorem physically before using it algebraically.

## Weak form

Define the residual.

Explain why an approximate solution generally does not make the strong equation zero at every point.

Define:

- weighting function;
- test function;
- trial function;
- admissible trial function;
- admissible test function.

Explain why the residual is multiplied by a weighting function.

Explain why it is integrated.

Derive the weak form one line at a time.

Show explicitly how one derivative is transferred from the temperature field to the test function.

Compare derivative orders before and after the operation.

Explain why the lower differentiability requirement leads to the term "weak form".

Show how the prescribed-flux boundary term arises naturally.

## Galerkin method

Do not write "use Galerkin's method" without defining it.

Explain separately:

$$
T^h=\text{trial approximation}
$$

and

$$
v^h=\text{test function}.
$$

Then explain why Galerkin's method chooses the same shape-function family for both.

If superscript h is used, explain what h means.

## Introduce the linear triangular element

Use a three-node triangle with nodes 1, 2, and 3.

Define:

- node;
- element;
- mesh;
- connectivity;
- degree of freedom;
- nodal temperature;
- interpolation;
- local element vector;
- global vector.

Use a clear diagram showing:

$$
(x_1,y_1),\quad (x_2,y_2),\quad (x_3,y_3).
$$

Begin with

$$
T(x,y)=a_1+a_2x+a_3y.
$$

Apply the three nodal conditions.

Show the resulting system in scalar form first.

Then write the matrix form.

Do not jump directly to standard shape functions.

Derive

$$
T=N_1T_1+N_2T_2+N_3T_3.
$$

Explain what each N_i does.

Prove:

$$
N_i(\text{node }j)=\delta_{ij}.
$$

If the Kronecker delta is introduced, define it.

Also derive:

$$
N_1+N_2+N_3=1.
$$

Explain why this is called partition of unity.

Explain the geometrical interpretation of triangular shape functions as area or barycentric coordinates.

## Introduce local or natural coordinates

Before introducing the Jacobian, explain why FEM often uses a simple reference element.

Use a reference triangle with coordinates such as

$$
(\xi,\eta)
$$

or another notation, but define the notation clearly and use it consistently.

Show both triangles:

Physical element: (x,y)

Reference element: (ξ,η)

Explain the purpose of the reference element:

- one standard geometry;
- simpler shape functions;
- simpler integration;
- a systematic mapping to any physical triangle.

Explain that

$$
x=x(\xi,\eta),\qquad y=y(\xi,\eta).
$$

Do not call this a coordinate transformation until the meaning has been explained.

## Derive the coordinate mapping

Use the same shape functions to map coordinates:

$$
x(\xi,\eta)=N_1x_1+N_2x_2+N_3x_3,
$$

$$
y(\xi,\eta)=N_1y_1+N_2y_2+N_3y_3.
$$

Explain why nodal coordinates are interpolated in this way.

If this is called an isoparametric formulation, define:

- "iso";
- "parametric";
- why the same shape functions are used for geometry and field interpolation.

Do not simply use the word isoparametric without explanation.

## Derive the Jacobian matrix from the mapping

Do not present the Jacobian as a memorised formula.

Start from:

$$
x=x(\xi,\eta),
$$

$$
y=y(\xi,\eta).
$$

Consider small changes:

$$
d\xi,\qquad d\eta.
$$

Show that the corresponding coordinate changes are

$$
dx=\frac{\partial x}{\partial \xi}d\xi+\frac{\partial x}{\partial \eta}d\eta,
$$

$$
dy=\frac{\partial y}{\partial \xi}d\xi+\frac{\partial y}{\partial \eta}d\eta.
$$

Then write these equations in matrix form.

Define the Jacobian matrix explicitly.

Use one consistent convention and state it clearly.

For example:

$$
[J]=
\begin{bmatrix}
\dfrac{\partial x}{\partial \xi} & \dfrac{\partial x}{\partial \eta}\\
\dfrac{\partial y}{\partial \xi} & \dfrac{\partial y}{\partial \eta}
\end{bmatrix}
$$

so that

$$
\begin{bmatrix}
dx\\dy
\end{bmatrix}
=
[J]
\begin{bmatrix}
d\xi\\d\eta
\end{bmatrix}.
$$

Explain that some books use the transpose convention.

If an uploaded textbook uses a different convention, show both and explain the difference.

## Explain each Jacobian entry

Derive:

$$
\frac{\partial x}{\partial \xi},\quad
\frac{\partial x}{\partial \eta},\quad
\frac{\partial y}{\partial \xi},\quad
\frac{\partial y}{\partial \eta}
$$

from the coordinate interpolation.

For example:

$$
x=N_1x_1+N_2x_2+N_3x_3.
$$

Then:

$$
\frac{\partial x}{\partial \xi}
=
\frac{\partial N_1}{\partial \xi}x_1
+
\frac{\partial N_2}{\partial \xi}x_2
+
\frac{\partial N_3}{\partial \xi}x_3.
$$

Show all four derivatives.

Then assemble them into [J].

State the dimensions:

$$
[J]:2\times2.
$$

## Explain what the Jacobian means physically and geometrically

Do not treat J as only a matrix used in software.

Explain that the Jacobian describes how a small change in reference coordinates maps to a small change in physical coordinates.

Explain:

- stretching;
- compression;
- rotation or skewing;
- orientation;
- local geometric scaling.

Then explain the determinant.

For

$$
[J]=
\begin{bmatrix}
J_{11}&J_{12}\\
J_{21}&J_{22}
\end{bmatrix},
$$

derive:

$$
\det[J]=J_{11}J_{22}-J_{12}J_{21}.
$$

Explain why det[J] must not be zero for a valid element mapping.

Explain what happens if

$$
\det[J]=0.
$$

Explain the meaning of a negative determinant and its connection to reversed node ordering or element orientation.

## Derive the area transformation

Do not simply state

$$
dA=\det[J]d\xi d\eta.
$$

Explain why area changes under a coordinate mapping.

Use the two differential coordinate vectors generated by dξ and dη.

Explain that their mapped vectors span a small parallelogram in physical space.

Show how the determinant measures its area scaling.

Then derive:

$$
dA=|\det[J]|d\xi d\eta
$$

or, for a consistently oriented element with positive determinant,

$$
dA=\det[J]d\xi d\eta.
$$

Explain which convention will be used in later integration.

## Derive the inverse Jacobian

Starting from

$$
\begin{bmatrix}
dx\\dy
\end{bmatrix}
=
[J]
\begin{bmatrix}
d\xi\\d\eta
\end{bmatrix},
$$

show how multiplication by [J]^{-1} gives

$$
\begin{bmatrix}
d\xi\\d\eta
\end{bmatrix}
=
[J]^{-1}
\begin{bmatrix}
dx\\dy
\end{bmatrix}.
$$

Derive the inverse of a symbolic 2×2 matrix before using it.

For

$$
[J]=
\begin{bmatrix}
a&b\\c&d
\end{bmatrix},
$$

derive:

$$
[J]^{-1}
=
\frac{1}{ad-bc}
\begin{bmatrix}
d&-b\\-c&a
\end{bmatrix}.
$$

Do not assume I already know why this is true.

Verify it by multiplication if useful.

## Derive derivative transformation with the chain rule

This is essential.

Do not jump from local derivatives to global derivatives.

First teach the chain rule for a function of more than one variable.

If

$$
N=N(\xi,\eta)
$$

and

$$
\xi=\xi(x,y),\qquad \eta=\eta(x,y),
$$

derive:

$$
\frac{\partial N}{\partial x}
=
\frac{\partial N}{\partial \xi}\frac{\partial \xi}{\partial x}
+
\frac{\partial N}{\partial \eta}\frac{\partial \eta}{\partial x},
$$

and

$$
\frac{\partial N}{\partial y}
=
\frac{\partial N}{\partial \xi}\frac{\partial \xi}{\partial y}
+
\frac{\partial N}{\partial \eta}\frac{\partial \eta}{\partial y}.
$$

Then write the matrix form.

Using the chosen Jacobian convention, derive the correct relationship between

$$
\begin{bmatrix}
N_{,x}\\N_{,y}
\end{bmatrix}
$$

and

$$
\begin{bmatrix}
N_{,\xi}\\N_{,\eta}
\end{bmatrix}.
$$

Do not guess whether J^{-1} or J^{-T} is required.

Derive the orientation carefully from the chain rule.

Explicitly state the final transformation consistent with the Jacobian convention used earlier.

This is especially important because different FEM books define J with different row and column conventions.

## Build the shape-function derivative matrix

For all element shape functions, collect the local derivatives:

$$
\frac{\partial N_i}{\partial \xi},\qquad
\frac{\partial N_i}{\partial \eta}.
$$

Then use the inverse Jacobian transformation to obtain:

$$
\frac{\partial N_i}{\partial x},\qquad
\frac{\partial N_i}{\partial y}.
$$

Show this for each node.

Only after that assemble the global-coordinate shape-function gradient matrix.

## Derive the B-matrix from first principles

Do not present [B] as a standard matrix.

Start from

$$
T=N_1T_1+N_2T_2+N_3T_3.
$$

Differentiate explicitly:

$$
\frac{\partial T}{\partial x}
=
\frac{\partial N_1}{\partial x}T_1
+
\frac{\partial N_2}{\partial x}T_2
+
\frac{\partial N_3}{\partial x}T_3,
$$

and

$$
\frac{\partial T}{\partial y}
=
\frac{\partial N_1}{\partial y}T_1
+
\frac{\partial N_2}{\partial y}T_2
+
\frac{\partial N_3}{\partial y}T_3.
$$

Then collect the equations:

$$
\begin{bmatrix}
\dfrac{\partial T}{\partial x}\\
\dfrac{\partial T}{\partial y}
\end{bmatrix}
=
[B]
\begin{bmatrix}
T_1\\T_2\\T_3
\end{bmatrix}
$$

with

$$
[B]
=
\begin{bmatrix}
\dfrac{\partial N_1}{\partial x}&\dfrac{\partial N_2}{\partial x}&\dfrac{\partial N_3}{\partial x}\\
\dfrac{\partial N_1}{\partial y}&\dfrac{\partial N_2}{\partial y}&\dfrac{\partial N_3}{\partial y}
\end{bmatrix}.
$$

Explain why this matrix is called B.

If the FEM textbook uses the term "gradient matrix", "derivative matrix", or another term, state that terminology.

State:

$$
[B]:2\times3.
$$

Then explain why [B]{T_e} produces a 2×1 temperature-gradient vector.

## Show the direct and Jacobian routes to B

For the three-node linear triangle, derive B in two ways.

### Route A: direct physical-coordinate derivation

Derive shape functions directly in terms of x and y.

Then differentiate them to obtain N_{i,x} and N_{i,y}.

Construct B.

### Route B: natural-coordinate/Jacobian derivation

Define the shape functions in ξ and η.

Calculate N_{i,ξ} and N_{i,η}.

Construct J.

Calculate J^{-1}.

Transform the derivatives:

$$
(N_{i,\xi},N_{i,\eta})
\rightarrow
(N_{i,x},N_{i,y}).
$$

Then construct B.

Show that both routes give the same result.

This comparison is mandatory because I want to understand what the Jacobian actually does rather than memorise it as a software operation.

## Explain why B is constant for a linear triangle

Show that the natural-coordinate derivatives of linear shape functions are constant.

Show that the Jacobian of a straight-sided three-node triangle is constant.

Therefore show that J^{-1} is constant.

Hence show that N_{i,x} and N_{i,y} are constant.

Therefore:

$$
[B]=\text{constant within the element}.
$$

Then show why:

$$
\nabla T=[B]\{T_e\}
$$

is constant inside one linear triangular element.

If D is constant, show why:

$$
\mathbf q=-[D][B]\{T_e\}
$$

is also constant inside that element.

## Explain the conductivity matrix D

Start from the component Fourier equations.

Then form:

$$
\mathbf q=-[D]\nabla T.
$$

For isotropic conduction, explain:

$$
[D]=
\begin{bmatrix}
k&0\\0&k
\end{bmatrix}
$$
