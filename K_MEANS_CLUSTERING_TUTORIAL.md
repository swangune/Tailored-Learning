---
title: "K-Means Clustering - Complete Tutorial"
subtitle: "CSCM445 Machine Learning | Computational Engineering Study Notes"
author: "Prepared from approved study session material"
date: "5 October 2026"
geometry: margin=22mm
fontsize: 11pt
header-includes:
  - \usepackage{amsmath,amssymb,booktabs,longtable,array}
  - \usepackage{microtype}
  - \usepackage{enumitem}
  - \setlist{nosep}
  - \usepackage{fancyhdr}
  - \pagestyle{fancy}
  - \fancyhf{}
  - \lhead{CSCM445 Machine Learning}
  - \rhead{K-Means Clustering}
  - \cfoot{\thepage}
  - \setlength{\headheight}{14pt}
toc: true
toc-depth: 2
numbersections: true
---

# Tutorial Scope and Source Basis

This tutorial formalises the complete K-means study carried out in the approved study session. It keeps Bishop's notation and the course topics, but explains every concept in direct, concrete terms before using the general equations.

The technical core follows Christopher M. Bishop, *Pattern Recognition and Machine Learning*, Section 9.1 and Section 9.1.1: observations $\mathbf{x}_n$, cluster centres $\boldsymbol{\mu}_k$, binary assignment variables $r_{nk}$, objective function $J$, alternating assignment and centre updates, convergence, initialization, limitations, image segmentation, and vector quantisation. The course material additionally identifies SSE, the elbow method, silhouette score, and Dunn's index as methods for choosing the number of clusters.

**Important convention.** The six-observation geyser dataset used for the main worked example is a teaching dataset based on the Old Faithful setting. It is not claimed to reproduce Bishop's exact Old Faithful values. All later arithmetic uses the rounded standardized coordinates shown in Section 7 consistently.

# What K-Means Is

K-means is an **unsupervised learning** algorithm. We have observations

$$
\mathbf{x}_1,\mathbf{x}_2,\ldots,\mathbf{x}_N,
$$

but we do not have supplied class labels telling the algorithm which group each observation belongs to. The task is to discover groups, called **clusters**.

If

$$K=2,$$

we ask the algorithm to find two clusters. If

$$K=10,$$

we ask it to find ten clusters. Therefore

$$
\boxed{K=\text{number of clusters}}
$$

and, because each cluster has a representative centre,

$$
\boxed{K=\text{number of cluster centres}}.
$$

For $K=3$, for example, the centres are

$$
\boldsymbol{\mu}_1,\qquad \boldsymbol{\mu}_2,\qquad \boldsymbol{\mu}_3.
$$

Bishop initially treats $K$ as a supplied value. Choosing a defensible value of $K$ is studied later in this tutorial.

# What an Observation Vector Means

Suppose a geyser eruption is described using two measured features:

- eruption duration in minutes;
- waiting time until the next eruption in minutes.

One observation can therefore be written

$$
\mathbf{x}_1=
\begin{bmatrix}
2.0\\
55
\end{bmatrix}.
$$

This means

$$2.0=\text{eruption duration in minutes}$$

and

$$55=\text{waiting time in minutes}.$$

Therefore

$$
\boxed{\mathbf{x}_n=\text{one observation}}
$$

and the numbers inside that vector are its **features**.

If there are $D$ features, then

$$
\mathbf{x}_n\in\mathbb{R}^D.
$$

Our main example has

$$
D=2.
$$

If all observations are placed into a conventional data table, they can be regarded as a matrix $\mathbf{X}$ in which rows are observations and columns are features. Bishop commonly writes an individual observation $\mathbf{x}_n$ as a column vector, so a table row can be thought of as $\mathbf{x}_n^T$.

# Euclidean Distance: Why Do We Subtract and Square?

K-means needs a way to decide whether an observation is near or far from a cluster centre. For two-dimensional data,

$$
\mathbf{x}=
\begin{bmatrix}x_1\\x_2\end{bmatrix},
\qquad
\boldsymbol{\mu}=
\begin{bmatrix}\mu_1\\\mu_2\end{bmatrix}.
$$

The squared Euclidean distance is

$$
\boxed{
\|\mathbf{x}-\boldsymbol{\mu}\|^2
=(x_1-\mu_1)^2+(x_2-\mu_2)^2
}.
$$

The subtraction $x_1-\mu_1$ measures the separation along feature 1. Likewise, $x_2-\mu_2$ measures separation along feature 2. Without subtraction there is no measure of how far the coordinates are from one another.

The square ensures that negative coordinate differences do not create negative distance contributions. For example,

$$x_1-\mu_1=-3$$

but

$$(-3)^2=9.$$

Hence

$$
\boxed{\text{subtraction measures coordinate separation}}
$$

and

$$
\boxed{\text{squaring makes every contribution non-negative}}.
$$

# Main Worked Dataset and Standardisation

The raw teaching dataset is

$$
\mathbf{x}_1=\begin{bmatrix}2.0\\55\end{bmatrix},\quad
\mathbf{x}_2=\begin{bmatrix}2.2\\58\end{bmatrix},\quad
\mathbf{x}_3=\begin{bmatrix}1.8\\52\end{bmatrix},
$$

$$
\mathbf{x}_4=\begin{bmatrix}4.2\\78\end{bmatrix},\quad
\mathbf{x}_5=\begin{bmatrix}4.4\\82\end{bmatrix},\quad
\mathbf{x}_6=\begin{bmatrix}4.6\\86\end{bmatrix}.
$$

Thus

$$N=6,\qquad D=2.$$

The eruption duration values are around 2--5, while the waiting-time values are around 50--90. Because K-means uses Euclidean distance, the numerically larger feature could dominate the distance merely because of scale. Bishop standardises the Old Faithful variables for this reason.

We use

$$
\boxed{z=\frac{x-\bar{x}}{\sigma}}.
$$

For this teaching example, the variance is calculated using division by $N$.

## Standardising Feature 1: Eruption Duration

The duration values are

$$2.0,\quad2.2,\quad1.8,\quad4.2,\quad4.4,\quad4.6.$$

The mean is

$$
\bar{x}_1=\frac{2.0+2.2+1.8+4.2+4.4+4.6}{6}.
$$

Add the values step by step:

$$2.0+2.2=4.2,$$

$$4.2+1.8=6.0,$$

$$6.0+4.2=10.2,$$

$$10.2+4.4=14.6,$$

$$14.6+4.6=19.2.$$

Therefore

$$
\bar{x}_1=\frac{19.2}{6}=\boxed{3.2}.
$$

Now calculate every deviation from the mean:

$$2.0-3.2=-1.2,$$

$$2.2-3.2=-1.0,$$

$$1.8-3.2=-1.4,$$

$$4.2-3.2=1.0,$$

$$4.4-3.2=1.2,$$

$$4.6-3.2=1.4.$$

Square every deviation:

$$(-1.2)^2=1.44,$$

$$(-1.0)^2=1.00,$$

$$(-1.4)^2=1.96,$$

$$1.0^2=1.00,$$

$$1.2^2=1.44,$$

$$1.4^2=1.96.$$

Add them:

$$1.44+1.00+1.96+1.00+1.44+1.96=8.80.$$

The variance is

$$
\sigma_1^2=\frac{8.8}{6}=1.4667.
$$

Hence the standard deviation is

$$
\sigma_1=\sqrt{1.4667}=\boxed{1.211\text{ approximately}}.
$$

## Standardising Feature 2: Waiting Time

The waiting-time values are

$$55,\quad58,\quad52,\quad78,\quad82,\quad86.$$

The mean is

$$
\bar{x}_2=\frac{55+58+52+78+82+86}{6}.
$$

Add them step by step:

$$55+58=113,$$

$$113+52=165,$$

$$165+78=243,$$

$$243+82=325,$$

$$325+86=411.$$

Therefore

$$
\bar{x}_2=\frac{411}{6}=\boxed{68.5}.
$$

Now calculate every deviation:

$$55-68.5=-13.5,$$

$$58-68.5=-10.5,$$

$$52-68.5=-16.5,$$

$$78-68.5=9.5,$$

$$82-68.5=13.5,$$

$$86-68.5=17.5.$$

Square them:

$$(-13.5)^2=182.25,$$

$$(-10.5)^2=110.25,$$

$$(-16.5)^2=272.25,$$

$$9.5^2=90.25,$$

$$13.5^2=182.25,$$

$$17.5^2=306.25.$$

Add:

$$182.25+110.25=292.50,$$

$$292.50+272.25=564.75,$$

$$564.75+90.25=655.00,$$

$$655.00+182.25=837.25,$$

$$837.25+306.25=1143.50.$$

Therefore

$$
\sigma_2^2=\frac{1143.5}{6}=190.5833
$$

and

$$
\sigma_2=\sqrt{190.5833}=\boxed{13.805\text{ approximately}}.
$$

## Standardise Every Observation

Use

$$z=\frac{x-\bar{x}}{\sigma}.$$

For $\mathbf{x}_1=(2.0,55)$:

$$
\frac{2.0-3.2}{1.211}=\frac{-1.2}{1.211}\approx-0.991,
$$

$$
\frac{55-68.5}{13.805}=\frac{-13.5}{13.805}\approx-0.978.
$$

Hence

$$
\boxed{\mathbf{x}_1=\begin{bmatrix}-0.991\\-0.978\end{bmatrix}}.
$$

For $\mathbf{x}_2=(2.2,58)$:

$$
\frac{2.2-3.2}{1.211}=\frac{-1.0}{1.211}\approx-0.826,
$$

$$
\frac{58-68.5}{13.805}=\frac{-10.5}{13.805}\approx-0.761.
$$

Therefore

$$
\boxed{\mathbf{x}_2=\begin{bmatrix}-0.826\\-0.761\end{bmatrix}}.
$$

For $\mathbf{x}_3=(1.8,52)$:

$$
\frac{1.8-3.2}{1.211}=\frac{-1.4}{1.211}\approx-1.156,
$$

$$
\frac{52-68.5}{13.805}=\frac{-16.5}{13.805}\approx-1.195.
$$

Therefore

$$
\boxed{\mathbf{x}_3=\begin{bmatrix}-1.156\\-1.195\end{bmatrix}}.
$$

For $\mathbf{x}_4=(4.2,78)$:

$$
\frac{4.2-3.2}{1.211}=\frac{1}{1.211}\approx0.826,
$$

$$
\frac{78-68.5}{13.805}=\frac{9.5}{13.805}\approx0.688.
$$

Therefore

$$
\boxed{\mathbf{x}_4=\begin{bmatrix}0.826\\0.688\end{bmatrix}}.
$$

For $\mathbf{x}_5=(4.4,82)$:

$$
\frac{4.4-3.2}{1.211}=\frac{1.2}{1.211}\approx0.991,
$$

$$
\frac{82-68.5}{13.805}=\frac{13.5}{13.805}\approx0.978.
$$

Therefore

$$
\boxed{\mathbf{x}_5=\begin{bmatrix}0.991\\0.978\end{bmatrix}}.
$$

For $\mathbf{x}_6=(4.6,86)$:

$$
\frac{4.6-3.2}{1.211}=\frac{1.4}{1.211}\approx1.156,
$$

$$
\frac{86-68.5}{13.805}=\frac{17.5}{13.805}\approx1.268.
$$

Therefore

$$
\boxed{\mathbf{x}_6=\begin{bmatrix}1.156\\1.268\end{bmatrix}}.
$$

All subsequent calculations use these rounded standardized values consistently.

# K-Means Assignment Variables and Objective Function

Bishop uses binary assignment variables

$$
r_{nk}\in\{0,1\}.
$$

If observation $n$ belongs to cluster $k$, then

$$r_{nk}=1.$$

For every other cluster, the indicator is zero. For example, with $K=2$, if $\mathbf{x}_3$ belongs to cluster 1, then

$$
r_{31}=1,\qquad r_{32}=0.
$$

This is a **hard 1-of-$K$ assignment**.

The K-means objective function is

$$
\boxed{
J=
\sum_{n=1}^{N}
\sum_{k=1}^{K}
r_{nk}\|\mathbf{x}_n-\boldsymbol{\mu}_k\|^2
}.
$$

In words,

$$
\boxed{J=\text{total squared distance of all observations from their assigned cluster centres}}.
$$

The goal is to make $J$ as small as possible.

## Meaning of $\arg\min$

The assignment rule can be written

$$
r_{nk}=
\begin{cases}
1, & k=\arg\min_j\|\mathbf{x}_n-\boldsymbol{\mu}_j\|^2,\\
0, & \text{otherwise}.
\end{cases}
$$

The word **minimum** asks for the smallest value. The expression **arg min** asks which argument or index produced that smallest value.

Suppose the squared distances from one observation to three centres are

$$5.2,\quad0.8,\quad4.1.$$

Then

$$\min=0.8,$$

but

$$\arg\min=2.$$

Therefore that observation is assigned to cluster 2.

# Initialisation for the Main $K=2$ Run

For

$$K=2,$$

we need two initial cluster centres. A practical procedure discussed by Bishop is to choose a random subset of $K$ observations as initial centres.

To avoid secretly selecting one point from each visually obvious group, suppose an uninformed random draw selects $\mathbf{x}_1$ and $\mathbf{x}_2$:

$$
\boldsymbol{\mu}_1^{(0)}=\mathbf{x}_1=
\begin{bmatrix}-0.991\\-0.978\end{bmatrix},
$$

$$
\boldsymbol{\mu}_2^{(0)}=\mathbf{x}_2=
\begin{bmatrix}-0.826\\-0.761\end{bmatrix}.
$$

These two starting points are actually fairly close to one another, so this is not an ideal initialization. That is useful because we can see K-means correct itself over several iterations.

The observation number itself is irrelevant to geometric separation. Choosing row 1 and row 57 does not guarantee well-separated centres. The quantity that matters is feature-space distance,

$$
\|\mathbf{x}_1-\mathbf{x}_{57}\|,
$$

not row-index separation $|1-57|$.

# Complete $K=2$ Worked Run

## Iteration 1: Assignment Step

Every observation must be compared with both centres.

### Observation $\mathbf{x}_1$

Distance to centre 1:

$$
\|\mathbf{x}_1-\boldsymbol{\mu}_1^{(0)}\|^2
=(-0.991+0.991)^2+(-0.978+0.978)^2=0.
$$

Distance to centre 2:

$$
(-0.991+0.826)^2+(-0.978+0.761)^2
$$

$$
=(-0.165)^2+(-0.217)^2
$$

$$
=0.027225+0.047089
$$

$$
=\boxed{0.074314}.
$$

Since $0<0.074314$,

$$
\boxed{\mathbf{x}_1\rightarrow C_1}.
$$

### Observation $\mathbf{x}_2$

Distance to centre 1:

$$
(-0.826+0.991)^2+(-0.761+0.978)^2
$$

$$
=0.165^2+0.217^2
$$

$$
=0.027225+0.047089
$$

$$
=\boxed{0.074314}.
$$

Distance to centre 2:

$$
(-0.826+0.826)^2+(-0.761+0.761)^2=\boxed{0}.
$$

Therefore

$$
\boxed{\mathbf{x}_2\rightarrow C_2}.
$$

### Observation $\mathbf{x}_3$

Distance to centre 1:

$$
(-1.156+0.991)^2+(-1.195+0.978)^2
$$

$$
=(-0.165)^2+(-0.217)^2
$$

$$
=0.027225+0.047089
$$

$$
=\boxed{0.074314}.
$$

Distance to centre 2:

$$
(-1.156+0.826)^2+(-1.195+0.761)^2
$$

$$
=(-0.330)^2+(-0.434)^2
$$

$$
=0.108900+0.188356
$$

$$
=\boxed{0.297256}.
$$

Therefore

$$
\boxed{\mathbf{x}_3\rightarrow C_1}.
$$

### Observation $\mathbf{x}_4$

Distance to centre 1:

$$
(0.826+0.991)^2+(0.688+0.978)^2
$$

$$
=1.817^2+1.666^2
$$

$$
=3.301489+2.775556
$$

$$
=\boxed{6.077045}.
$$

Distance to centre 2:

$$
(0.826+0.826)^2+(0.688+0.761)^2
$$

$$
=1.652^2+1.449^2
$$

$$
=2.729104+2.099601
$$

$$
=\boxed{4.828705}.
$$

Therefore

$$
\boxed{\mathbf{x}_4\rightarrow C_2}.
$$

### Observation $\mathbf{x}_5$

Distance to centre 1:

$$
(0.991+0.991)^2+(0.978+0.978)^2
$$

$$
=1.982^2+1.956^2
$$

$$
=3.928324+3.825936
$$

$$
=\boxed{7.754260}.
$$

Distance to centre 2:

$$
(0.991+0.826)^2+(0.978+0.761)^2
$$

$$
=1.817^2+1.739^2
$$

$$
=3.301489+3.024121
$$

$$
=\boxed{6.325610}.
$$

Therefore

$$
\boxed{\mathbf{x}_5\rightarrow C_2}.
$$

### Observation $\mathbf{x}_6$

Distance to centre 1:

$$
(1.156+0.991)^2+(1.268+0.978)^2
$$

$$
=2.147^2+2.246^2
$$

$$
=4.609609+5.044516
$$

$$
=\boxed{9.654125}.
$$

Distance to centre 2:

$$
(1.156+0.826)^2+(1.268+0.761)^2
$$

$$
=1.982^2+2.029^2
$$

$$
=3.928324+4.116841
$$

$$
=\boxed{8.045165}.
$$

Therefore

$$
\boxed{\mathbf{x}_6\rightarrow C_2}.
$$

After iteration 1,

$$
\boxed{C_1=\{\mathbf{x}_1,\mathbf{x}_3\}}
$$

and

$$
\boxed{C_2=\{\mathbf{x}_2,\mathbf{x}_4,\mathbf{x}_5,\mathbf{x}_6\}}.
$$

The hard assignment indicators are

\begin{center}
\begin{tabular}{c|cc}
\toprule
Observation & $r_{n1}$ & $r_{n2}$\\
\midrule
$\mathbf{x}_1$ & 1 & 0\\
$\mathbf{x}_2$ & 0 & 1\\
$\mathbf{x}_3$ & 1 & 0\\
$\mathbf{x}_4$ & 0 & 1\\
$\mathbf{x}_5$ & 0 & 1\\
$\mathbf{x}_6$ & 0 & 1\\
\bottomrule
\end{tabular}
\end{center}

## Iteration 1: Centre Update

Bishop's update equation is

$$
\boxed{
\boldsymbol{\mu}_k=
\frac{\sum_n r_{nk}\mathbf{x}_n}{\sum_n r_{nk}}
}.
$$

The numerator adds observations belonging to cluster $k$; the denominator counts them. Therefore the centre becomes their mean.

For cluster 1,

$$
\boldsymbol{\mu}_1^{(1)}=\frac{\mathbf{x}_1+\mathbf{x}_3}{2}.
$$

First coordinate:

$$
\frac{-0.991-1.156}{2}=\frac{-2.147}{2}=-1.0735.
$$

Second coordinate:

$$
\frac{-0.978-1.195}{2}=\frac{-2.173}{2}=-1.0865.
$$

Therefore

$$
\boxed{
\boldsymbol{\mu}_1^{(1)}=
\begin{bmatrix}-1.0735\\-1.0865\end{bmatrix}
}.
$$

For cluster 2,

$$
\boldsymbol{\mu}_2^{(1)}=
\frac{\mathbf{x}_2+\mathbf{x}_4+\mathbf{x}_5+\mathbf{x}_6}{4}.
$$

First coordinate:

$$
-0.826+0.826+0.991+1.156=2.147,
$$

so

$$
\frac{2.147}{4}=0.53675.
$$

Second coordinate:

$$
-0.761+0.688+0.978+1.268=2.173,
$$

so

$$
\frac{2.173}{4}=0.54325.
$$

Therefore

$$
\boxed{
\boldsymbol{\mu}_2^{(1)}=
\begin{bmatrix}0.53675\\0.54325\end{bmatrix}
}.
$$

## Iteration 2: Assignment Step

Current centres are

$$
\boldsymbol{\mu}_1=(-1.0735,-1.0865),
\qquad
\boldsymbol{\mu}_2=(0.53675,0.54325).
$$

### Observation $\mathbf{x}_1$

To centre 1:

$$
(-0.991+1.0735)^2+(-0.978+1.0865)^2
$$

$$
=0.0825^2+0.1085^2
$$

$$
=0.00680625+0.01177225
$$

$$
=\boxed{0.0185785}.
$$

To centre 2:

$$
(-0.991-0.53675)^2+(-0.978-0.54325)^2
$$

$$
=(-1.52775)^2+(-1.52125)^2
$$

$$
=2.33402006+2.31420156
$$

$$
=\boxed{4.64822162}.
$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Observation $\mathbf{x}_2$

To centre 1:

$$
(-0.826+1.0735)^2+(-0.761+1.0865)^2
$$

$$
=0.2475^2+0.3255^2
$$

$$
=0.06125625+0.10595025
$$

$$
=\boxed{0.1672065}.
$$

To centre 2:

$$
(-0.826-0.53675)^2+(-0.761-0.54325)^2
$$

$$
=(-1.36275)^2+(-1.30425)^2
$$

$$
=1.85708756+1.70106806
$$

$$
=\boxed{3.55815562}.
$$

Therefore

$$
\boxed{\mathbf{x}_2:C_2\rightarrow C_1}.
$$

This observation changes cluster.

### Observation $\mathbf{x}_3$
To centre 1:

$$
(-1.156+1.0735)^2+(-1.195+1.0865)^2
$$

$$
=(-0.0825)^2+(-0.1085)^2
$$

$$
=\boxed{0.0185785}.
$$

To centre 2:

$$
(-1.156-0.53675)^2+(-1.195-0.54325)^2
$$

$$
=(-1.69275)^2+(-1.73825)^2
$$

$$
=2.86540256+3.02151306
$$

$$
=\boxed{5.88691562}.
$$

Therefore $\mathbf{x}_3\rightarrow C_1$.

### Observation $\mathbf{x}_4$

To centre 1:

$$
(0.826+1.0735)^2+(0.688+1.0865)^2
$$

$$
=1.8995^2+1.7745^2
$$

$$
=3.60810025+3.14885025
$$

$$
=\boxed{6.7569505}.
$$

To centre 2:

$$
(0.826-0.53675)^2+(0.688-0.54325)^2
$$

$$
=0.28925^2+0.14475^2
$$

$$
=0.08366556+0.02095256
$$

$$
=\boxed{0.10461812}.
$$

Therefore $\mathbf{x}_4\rightarrow C_2$.

### Observation $\mathbf{x}_5$

To centre 1:

$$
(0.991+1.0735)^2+(0.978+1.0865)^2
$$

$$
=2.0645^2+2.0645^2
$$

$$
=4.26216025+4.26216025
$$

$$
=\boxed{8.5243205}.
$$

To centre 2:

$$
(0.991-0.53675)^2+(0.978-0.54325)^2
$$

$$
=0.45425^2+0.43475^2
$$

$$
=0.20634306+0.18900756
$$

$$
=\boxed{0.39535062}.
$$

Therefore $\mathbf{x}_5\rightarrow C_2$.

### Observation $\mathbf{x}_6$

To centre 1:

$$
(1.156+1.0735)^2+(1.268+1.0865)^2
$$

$$
=2.2295^2+2.3545^2
$$

$$
=4.97067025+5.54367025
$$

$$
=\boxed{10.5143405}.
$$

To centre 2:

$$
(1.156-0.53675)^2+(1.268-0.54325)^2
$$

$$
=0.61925^2+0.72475^2
$$

$$
=0.38347056+0.52526256
$$

$$
=\boxed{0.90873312}.
$$

Therefore $\mathbf{x}_6\rightarrow C_2$.

After iteration 2,

$$
\boxed{C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\}}
$$

and

$$
\boxed{C_2=\{\mathbf{x}_4,\mathbf{x}_5,\mathbf{x}_6\}}.
$$

## Iteration 2: Centre Update

For cluster 1,

$$
\boldsymbol{\mu}_1^{(2)}=
\frac{\mathbf{x}_1+\mathbf{x}_2+\mathbf{x}_3}{3}.
$$

First coordinate:

$$
-0.991-0.826-1.156=-2.973,
$$

$$
\frac{-2.973}{3}=-0.991.
$$

Second coordinate:

$$
-0.978-0.761-1.195=-2.934,
$$

$$
\frac{-2.934}{3}=-0.978.
$$

Therefore

$$
\boxed{\boldsymbol{\mu}_1^{(2)}=(-0.991,-0.978)}.
$$

For cluster 2,

$$
0.826+0.991+1.156=2.973,
$$

$$
\frac{2.973}{3}=0.991,
$$

and

$$
0.688+0.978+1.268=2.934,
$$

$$
\frac{2.934}{3}=0.978.
$$

Therefore

$$
\boxed{\boldsymbol{\mu}_2^{(2)}=(0.991,0.978)}.
$$

## Iteration 3: Check for Convergence

We must not simply assume the current grouping is final. Every observation is checked again against both updated centres.

### Observation $\mathbf{x}_1$

To centre 1:

$$d_1^2=0.$$

To centre 2:

$$
(-1.982)^2+(-1.956)^2
$$

$$
=3.928324+3.825936
$$

$$
=\boxed{7.754260}.
$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Observation $\mathbf{x}_2$

To centre 1:

$$
0.165^2+0.217^2
=0.027225+0.047089
=\boxed{0.074314}.
$$

To centre 2:

$$
(-1.817)^2+(-1.739)^2
$$

$$
=3.301489+3.024121
$$

$$
=\boxed{6.325610}.
$$

Therefore $\mathbf{x}_2\rightarrow C_1$.

### Observation $\mathbf{x}_3$

To centre 1:

$$
(-0.165)^2+(-0.217)^2
=\boxed{0.074314}.
$$

To centre 2:

$$
(-2.147)^2+(-2.173)^2
$$

$$
=4.609609+4.721929
$$

$$
=\boxed{9.331538}.
$$

Therefore $\mathbf{x}_3\rightarrow C_1$.

### Observation $\mathbf{x}_4$

To centre 1:

$$
1.817^2+1.666^2
$$

$$
=3.301489+2.775556
$$

$$
=\boxed{6.077045}.
$$

To centre 2:

$$
(-0.165)^2+(-0.290)^2
$$

$$
=0.027225+0.084100
$$

$$
=\boxed{0.111325}.
$$

Therefore $\mathbf{x}_4\rightarrow C_2$.

### Observation $\mathbf{x}_5$

To centre 1:

$$
1.982^2+1.956^2
=\boxed{7.754260}.
$$

To centre 2:

$$d_2^2=0.$$

Therefore $\mathbf{x}_5\rightarrow C_2$.

### Observation $\mathbf{x}_6$

To centre 1:

$$
2.147^2+2.246^2
=4.609609+5.044516
=\boxed{9.654125}.
$$

To centre 2:

$$
0.165^2+0.290^2
=0.027225+0.084100
=\boxed{0.111325}.
$$

Therefore $\mathbf{x}_6\rightarrow C_2$.

No observation changes cluster.

# Convergence

In plain language,

$$
\boxed{\text{convergence}=\text{the iterative process has settled}}.
$$

For K-means, the usual practical condition is that the assignments stop changing. Here the clusters remain

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\}
$$

and

$$
C_2=\{\mathbf{x}_4,\mathbf{x}_5,\mathbf{x}_6\}.
$$

Because the members do not change, the means also stay the same. Recalculating gives

$$
\boldsymbol{\mu}_1=(-0.991,-0.978)
$$

and

$$
\boldsymbol{\mu}_2=(0.991,0.978).
$$

Therefore

$$
\boxed{\text{K-means has converged}}.
$$

Bishop notes that one can also stop if a maximum number of iterations is reached. Each assignment and centre-update phase does not increase the objective, so the procedure converges. However,

$$
\boxed{\text{converged does not necessarily mean globally best}}.
$$

That distinction is studied later.

# Final Objective Value $J$ for the $K=2$ Solution

The final clusters are

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\},
\qquad
C_2=\{\mathbf{x}_4,\mathbf{x}_5,\mathbf{x}_6\}.
$$

The objective is

$$
J=\sum_{n=1}^{N}\sum_{k=1}^{K}r_{nk}\|\mathbf{x}_n-\boldsymbol{\mu}_k\|^2.
$$

For $\mathbf{x}_1$,

$$
1(0)+0(7.754260)=\boxed{0}.
$$

For $\mathbf{x}_2$,

$$
1(0.074314)+0(6.325610)=\boxed{0.074314}.
$$

For $\mathbf{x}_3$,

$$
1(0.074314)+0(9.331538)=\boxed{0.074314}.
$$

For $\mathbf{x}_4$,

$$
0(6.077045)+1(0.111325)=\boxed{0.111325}.
$$

For $\mathbf{x}_5$,

$$
0(7.754260)+1(0)=\boxed{0}.
$$

For $\mathbf{x}_6$,

$$
0(9.654125)+1(0.111325)=\boxed{0.111325}.
$$

Add all contributions:

$$
J=0+0.074314+0.074314+0.111325+0+0.111325.
$$

First,

$$0.074314+0.074314=0.148628.$$

Also,

$$0.111325+0.111325=0.222650.$$

Therefore

$$
J=0.148628+0.222650=\boxed{0.371278}.
$$

# Why the Cluster Centre Is the Mean

The mean is not chosen because it merely seems reasonable. It is the value that minimizes the sum of squared distances for a fixed cluster assignment.

Take only the eruption durations

$$2.0,\quad2.2,\quad1.8.$$

Try centre $\mu=1.8$:

$$
J=(2.0-1.8)^2+(2.2-1.8)^2+(1.8-1.8)^2
$$

$$
=0.2^2+0.4^2+0^2
$$

$$
=0.04+0.16+0
$$

$$
=\boxed{0.20}.
$$

Try $\mu=1.9$:

$$
J=(2.0-1.9)^2+(2.2-1.9)^2+(1.8-1.9)^2
$$

$$
=0.1^2+0.3^2+(-0.1)^2
$$

$$
=0.01+0.09+0.01
$$

$$
=\boxed{0.11}.
$$

Try $\mu=2.0$:

$$
J=(2.0-2.0)^2+(2.2-2.0)^2+(1.8-2.0)^2
$$

$$
=0^2+0.2^2+(-0.2)^2
$$

$$
=0+0.04+0.04
$$

$$
=\boxed{0.08}.
$$

Try $\mu=2.1$:

$$
J=(-0.1)^2+(0.1)^2+(-0.3)^2
$$

$$
=0.01+0.01+0.09
$$

$$
=\boxed{0.11}.
$$

Thus among these candidate centres the minimum occurs at $\mu=2.0$. The ordinary mean is

$$
\frac{2.0+2.2+1.8}{3}=\frac{6.0}{3}=\boxed{2.0}.
$$

## Deriving the Mean by Differentiation

Define

$$
J(\mu)=(2.0-\mu)^2+(2.2-\mu)^2+(1.8-\mu)^2.
$$

Differentiate with respect to $\mu$. Start with one term:

$$
\frac{d}{d\mu}(2.0-\mu)^2.
$$

By the chain rule,

$$
\frac{d}{d\mu}(2.0-\mu)^2
=2(2.0-\mu)\frac{d}{d\mu}(2.0-\mu).
$$

Because

$$
\frac{d}{d\mu}(2.0-\mu)=-1,
$$

we obtain

$$
-2(2.0-\mu)=2(\mu-2.0).
$$

Applying the same differentiation to all three terms,

$$
\frac{dJ}{d\mu}
=2(\mu-2.0)+2(\mu-2.2)+2(\mu-1.8).
$$

At the minimum, set the derivative equal to zero:

$$
2(\mu-2.0)+2(\mu-2.2)+2(\mu-1.8)=0.
$$

Divide by 2:

$$
(\mu-2.0)+(\mu-2.2)+(\mu-1.8)=0.
$$

Expand:

$$
\mu-2.0+\mu-2.2+\mu-1.8=0.
$$

Collect terms:

$$
3\mu-6=0.
$$

Therefore

$$3\mu=6$$

and

$$
\boxed{\mu=2}.
$$

For a general cluster containing $m$ one-dimensional observations,

$$
J(\mu_k)=\sum_n(x_n-\mu_k)^2.
$$

Differentiate:

$$
\frac{dJ}{d\mu_k}=2\sum_n(\mu_k-x_n).
$$

Set to zero:

$$
2\sum_n(\mu_k-x_n)=0.
$$

Remove the factor 2:

$$
\sum_n(\mu_k-x_n)=0.
$$

If there are $m$ points,

$$
m\mu_k-\sum_nx_n=0.
$$

Hence

$$
m\mu_k=\sum_nx_n
$$

and

$$
\boxed{\mu_k=\frac{\sum_nx_n}{m}}.
$$

With Bishop's assignment indicator,

$$
\boxed{
\boldsymbol{\mu}_k=
\frac{\sum_n r_{nk}\mathbf{x}_n}{\sum_n r_{nk}}
}.
$$

The denominator counts how many observations are assigned to cluster $k$, while the numerator adds those observations. Thus the centre is exactly their mean. This is the reason for the name **K-means**.

# Local Minimum Versus Global Minimum

A **global minimum** is the smallest possible objective value over all possible solutions. A **local minimum** is a solution that the iterative algorithm can no longer improve from its current state even though a better solution exists elsewhere.

Use the one-dimensional teaching data

$$50,\quad51,\quad52,\quad70,\quad71,\quad90$$

with

$$K=2.$$

## Run A: Unlucky Initialisation

Let

$$
\mu_1^{(0)}=70,\qquad \mu_2^{(0)}=90.
$$

For 50:

$$
(50-70)^2=400,
$$

$$
(50-90)^2=1600.
$$

Hence $50\rightarrow C_1$.

For 51:

$$
(51-70)^2=361,
$$

$$
(51-90)^2=1521.
$$

Hence $51\rightarrow C_1$.

For 52:

$$
(52-70)^2=324,
$$

$$
(52-90)^2=1444.
$$

Hence $52\rightarrow C_1$.

For 70:

$$
(70-70)^2=0,
$$

$$
(70-90)^2=400.
$$

Hence $70\rightarrow C_1$.

For 71:

$$
(71-70)^2=1,
$$

$$
(71-90)^2=361.
$$

Hence $71\rightarrow C_1$.

For 90:

$$
(90-70)^2=400,
$$

$$
(90-90)^2=0.
$$

Hence $90\rightarrow C_2$.

So

$$
C_1=\{50,51,52,70,71\},
\qquad
C_2=\{90\}.
$$

Update centre 1:

$$
\mu_1^{(1)}=\frac{50+51+52+70+71}{5}.
$$

Add:

$$50+51=101,$$

$$101+52=153,$$

$$153+70=223,$$

$$223+71=294.$$

Therefore

$$
\mu_1^{(1)}=\frac{294}{5}=\boxed{58.8}.
$$

For cluster 2,

$$
\mu_2^{(1)}=90.
$$

Check every point again.

For 50:

$$
(50-58.8)^2=(-8.8)^2=77.44,
$$

$$
(50-90)^2=1600,
$$

so $50\rightarrow C_1$.

For 51:

$$
(51-58.8)^2=(-7.8)^2=60.84,
$$

$$
(51-90)^2=1521,
$$

so $51\rightarrow C_1$.

For 52:

$$
(52-58.8)^2=(-6.8)^2=46.24,
$$

$$
(52-90)^2=1444,
$$

so $52\rightarrow C_1$.

For 70:

$$
(70-58.8)^2=11.2^2=125.44,
$$

$$
(70-90)^2=400,
$$

so $70\rightarrow C_1$.

For 71:

$$
(71-58.8)^2=12.2^2=148.84,
$$

$$
(71-90)^2=361,
$$

so $71\rightarrow C_1$.

For 90:

$$
(90-58.8)^2=31.2^2=973.44,
$$

$$
(90-90)^2=0,
$$

so $90\rightarrow C_2$.

No assignments change, so this run has converged.

Its objective is

$$
77.44+60.84+46.24+125.44+148.84+0.
$$

Add:

$$77.44+60.84=138.28,$$

$$138.28+46.24=184.52,$$

$$184.52+125.44=309.96,$$

$$309.96+148.84=458.80.$$

Therefore

$$
\boxed{J_A=458.8}.
$$

## Run B: Different Initialisation

Now use

$$
\mu_1^{(0)}=50,\qquad\mu_2^{(0)}=90.
$$

For 50:

$$0<1600,$$

so $50\rightarrow C_1$.

For 51:

$$1<1521,$$

so $51\rightarrow C_1$.

For 52:

$$4<1444,$$

so $52\rightarrow C_1$.

For 70:

$$
(70-50)^2=400,
$$

$$
(70-90)^2=400.
$$

This is a tie. Suppose the implementation resolves the tie by assigning the point to $C_1$.

For 71:

$$
(71-50)^2=441,
$$

$$
(71-90)^2=361,
$$

so $71\rightarrow C_2$.

For 90:

$$1600>0,$$

so $90\rightarrow C_2$.

Therefore

$$
C_1=\{50,51,52,70\},
\qquad
C_2=\{71,90\}.
$$

Update centre 1:

$$
\mu_1=\frac{50+51+52+70}{4}=\frac{223}{4}=55.75.
$$

Update centre 2:

$$\mu_2=\frac{71+90}{2}=80.5.
$$

Reassign every point.

For 50:

$$
(50-55.75)^2=(-5.75)^2=33.0625,
$$

$$
(50-80.5)^2=(-30.5)^2=930.25.
$$

Hence $50\rightarrow C_1$.

For 51:

$$
(51-55.75)^2=(-4.75)^2=22.5625,
$$

$$
(51-80.5)^2=(-29.5)^2=870.25.
$$

Hence $51\rightarrow C_1$.

For 52:

$$
(52-55.75)^2=(-3.75)^2=14.0625,
$$

$$
(52-80.5)^2=(-28.5)^2=812.25.
$$

Hence $52\rightarrow C_1$.

For 70:

$$
(70-55.75)^2=14.25^2=203.0625,
$$

$$
(70-80.5)^2=(-10.5)^2=110.25.
$$

Hence $70\rightarrow C_2$.

For 71:

$$
(71-55.75)^2=15.25^2=232.5625,
$$

$$
(71-80.5)^2=(-9.5)^2=90.25.
$$

Hence $71\rightarrow C_2$.

For 90:

$$
(90-55.75)^2=34.25^2=1173.0625,
$$

$$
(90-80.5)^2=9.5^2=90.25.
$$

Hence $90\rightarrow C_2$.

The clusters are now

$$
C_1=\{50,51,52\},
\qquad
C_2=\{70,71,90\}.
$$

Update again:

$$
\mu_1=\frac{50+51+52}{3}=\frac{153}{3}=\boxed{51},
$$

$$
\mu_2=\frac{70+71+90}{3}=\frac{231}{3}=\boxed{77}.
$$

The final objective for cluster 1 is

$$
(50-51)^2+(51-51)^2+(52-51)^2
$$

$$
=1+0+1=2.
$$

For cluster 2,

$$
(70-77)^2+(71-77)^2+(90-77)^2
$$

$$
=(-7)^2+(-6)^2+13^2
$$

$$
=49+36+169=254.
$$

Therefore

$$
\boxed{J_B=2+254=256}.
$$

Compare:

$$256<458.8.$$

Thus Run A converged, but it converged to a worse local solution. The example shows why initialization matters and why convergence alone does not prove that the global minimum has been found.

# Choosing the Number of Clusters $K$

The course material identifies four tools for selecting or validating $K$:

- SSE;
- the elbow method;
- silhouette score;
- Dunn's index.

For K-means, SSE is the same basic quantity as Bishop's objective $J$: the sum of squared distances from observations to their assigned cluster centres.

A critical fact is that SSE normally decreases as $K$ increases. At the extreme, if

$$K=N,$$

every observation can become its own cluster centre. Then every assigned distance is zero and therefore

$$SSE=0.$$

So selecting the $K$ that simply gives the smallest SSE would always encourage too many clusters. We need to examine whether additional clusters provide a meaningful improvement.

# Elbow Method: $K=1$

With one cluster, every observation belongs to the same cluster. Because the standardized features have zero mean, the centre is

$$
\boldsymbol{\mu}=\begin{bmatrix}0\\0\end{bmatrix}.
$$

For $\mathbf{x}_1$:

$$
(-0.991)^2+(-0.978)^2
$$

$$
=0.982081+0.956484
$$

$$
=\boxed{1.938565}.
$$

For $\mathbf{x}_2$:

$$
(-0.826)^2+(-0.761)^2
$$

$$
=0.682276+0.579121
$$

$$
=\boxed{1.261397}.
$$

For $\mathbf{x}_3$:

$$
(-1.156)^2+(-1.195)^2
$$

$$
=1.336336+1.428025
$$

$$
=\boxed{2.764361}.
$$

For $\mathbf{x}_4$:

$$
0.826^2+0.688^2
$$

$$
=0.682276+0.473344
$$

$$
=\boxed{1.155620}.
$$

For $\mathbf{x}_5$:

$$
0.991^2+0.978^2
$$

$$
=0.982081+0.956484
$$

$$
=\boxed{1.938565}.
$$

For $\mathbf{x}_6$:

$$
1.156^2+1.268^2
$$

$$
=1.336336+1.607824
$$

$$
=\boxed{2.944160}.
$$

Add every contribution:

$$
SSE_{K=1}=1.938565+1.261397+2.764361+1.155620+1.938565+2.944160.
$$

Step by step,

$$1.938565+1.261397=3.199962,$$

$$3.199962+2.764361=5.964323,$$

$$5.964323+1.155620=7.119943,$$

$$7.119943+1.938565=9.058508,$$

$$9.058508+2.944160=12.002668.$$

Therefore

$$
\boxed{SSE_{K=1}=12.002668}.
$$

For $K=2$, the complete K-means run already gave

$$
\boxed{SSE_{K=2}=0.371278}.
$$

The reduction is

$$
12.002668-0.371278=\boxed{11.631390}.
$$

That is a very large improvement.

# Complete $K=3$ Run for the Elbow Analysis

Now set

$$K=3.$$

Suppose the random initialization selects

$$
\boldsymbol{\mu}_1^{(0)}=\mathbf{x}_1=(-0.991,-0.978),
$$

$$
\boldsymbol{\mu}_2^{(0)}=\mathbf{x}_5=(0.991,0.978),
$$

$$
\boldsymbol{\mu}_3^{(0)}=\mathbf{x}_6=(1.156,1.268).
$$

## $K=3$, Iteration 1

### Observation $\mathbf{x}_1$

To centre 1:

$$d_1^2=0.$$

To centre 2:

$$
(-0.991-0.991)^2+(-0.978-0.978)^2
$$

$$
=(-1.982)^2+(-1.956)^2
$$

$$
=3.928324+3.825936
$$

$$
=\boxed{7.754260}.
$$

To centre 3:

$$
(-0.991-1.156)^2+(-0.978-1.268)^2
$$

$$
=(-2.147)^2+(-2.246)^2
$$

$$
=4.609609+5.044516
$$

$$
=\boxed{9.654125}.
$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Observation $\mathbf{x}_2$

To centre 1:

$$
0.165^2+0.217^2=\boxed{0.074314}.
$$

To centre 2:

$$
(-1.817)^2+(-1.739)^2
$$

$$
=3.301489+3.024121
$$

$$
=\boxed{6.325610}.
$$

To centre 3:

$$
(-1.982)^2+(-2.029)^2
$$

$$
=3.928324+4.116841
$$

$$
=\boxed{8.045165}.
$$

Therefore $\mathbf{x}_2\rightarrow C_1$.

### Observation $\mathbf{x}_3$

To centre 1:

$$
(-0.165)^2+(-0.217)^2=\boxed{0.074314}.
$$

To centre 2:

$$
(-2.147)^2+(-2.173)^2
$$

$$
=4.609609+4.721929
$$

$$
=\boxed{9.331538}.
$$

To centre 3:

$$
(-2.312)^2+(-2.463)^2
$$

$$
=5.345344+6.066369
$$

$$
=\boxed{11.411713}.
$$

Therefore $\mathbf{x}_3\rightarrow C_1$.

### Observation $\mathbf{x}_4$

To centre 1:

$$
1.817^2+1.666^2=\boxed{6.077045}.
$$

To centre 2:

$$
(-0.165)^2+(-0.290)^2
$$

$$
=0.027225+0.084100
$$

$$
=\boxed{0.111325}.
$$

To centre 3:

$$
(-0.330)^2+(-0.580)^2
$$

$$
=0.108900+0.336400
$$

$$
=\boxed{0.445300}.
$$

Therefore $\mathbf{x}_4\rightarrow C_2$.

### Observation $\mathbf{x}_5$

To centre 1:

$$
1.982^2+1.956^2=\boxed{7.754260}.
$$

To centre 2:

$$d_2^2=0.$$

To centre 3:

$$
(-0.165)^2+(-0.290)^2=\boxed{0.111325}.
$$

Therefore $\mathbf{x}_5\rightarrow C_2$.

### Observation $\mathbf{x}_6$

To centre 1:

$$
2.147^2+2.246^2=\boxed{9.654125}.
$$

To centre 2:

$$
0.165^2+0.290^2=\boxed{0.111325}.
$$

To centre 3:

$$d_3^2=0.$$

Therefore $\mathbf{x}_6\rightarrow C_3$.

The first $K=3$ grouping is

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\},
$$

$$
C_2=\{\mathbf{x}_4,\mathbf{x}_5\},
$$

$$
C_3=\{\mathbf{x}_6\}.
$$

## $K=3$, Centre Update

For cluster 1,

$$
\boldsymbol{\mu}_1^{(1)}=\frac{\mathbf{x}_1+\mathbf{x}_2+\mathbf{x}_3}{3}=(-0.991,-0.978).
$$

For cluster 2, first coordinate:

$$
\frac{0.826+0.991}{2}=\frac{1.817}{2}=0.9085.
$$

Second coordinate:

$$
\frac{0.688+0.978}{2}=\frac{1.666}{2}=0.833.
$$

Therefore

$$
\boldsymbol{\mu}_2^{(1)}=(0.9085,0.833).
$$

Cluster 3 contains only $\mathbf{x}_6$, so

$$
\boldsymbol{\mu}_3^{(1)}=(1.156,1.268).
$$

## $K=3$, Iteration 2

### Observation $\mathbf{x}_1$

To centre 1:

$$0.$$

To centre 2:

$$
(-0.991-0.9085)^2+(-0.978-0.833)^2
$$

$$
=(-1.8995)^2+(-1.811)^2
$$

$$
=3.60810025+3.279721
$$

$$
=\boxed{6.88782125}.
$$

To centre 3:

$$\boxed{9.654125}.$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Observation $\mathbf{x}_2$

To centre 1:

$$\boxed{0.074314}.$$

To centre 2:

$$
(-0.826-0.9085)^2+(-0.761-0.833)^2
$$

$$
=(-1.7345)^2+(-1.594)^2
$$

$$
=3.00849025+2.540836
$$

$$
=\boxed{5.54932625}.
$$

To centre 3:

$$\boxed{8.045165}.$$

Therefore $\mathbf{x}_2\rightarrow C_1$.

### Observation $\mathbf{x}_3$

To centre 1:

$$\boxed{0.074314}.$$

To centre 2:

$$
(-1.156-0.9085)^2+(-1.195-0.833)^2
$$

$$
=(-2.0645)^2+(-2.028)^2
$$

$$
=4.26216025+4.112784
$$

$$
=\boxed{8.37494425}.
$$

To centre 3:

$$\boxed{11.411713}.$$

Therefore $\mathbf{x}_3\rightarrow C_1$.

### Observation $\mathbf{x}_4$

To centre 1:

$$\boxed{6.077045}.$$

To centre 2:

$$
(0.826-0.9085)^2+(0.688-0.833)^2
$$

$$
=(-0.0825)^2+(-0.145)^2
$$

$$
=0.00680625+0.021025
$$

$$
=\boxed{0.02783125}.
$$

To centre 3:

$$\boxed{0.445300}.$$

Therefore $\mathbf{x}_4\rightarrow C_2$.

### Observation $\mathbf{x}_5$

To centre 1:

$$\boxed{7.754260}.$$

To centre 2:

$$
(0.991-0.9085)^2+(0.978-0.833)^2
$$

$$
=0.0825^2+0.145^2
$$

$$
=0.00680625+0.021025
$$

$$
=\boxed{0.02783125}.
$$

To centre 3:

$$\boxed{0.111325}.$$

Therefore $\mathbf{x}_5\rightarrow C_2$.

### Observation $\mathbf{x}_6$

To centre 1:

$$\boxed{9.654125}.$$

To centre 2:

$$
(1.156-0.9085)^2+(1.268-0.833)^2
$$

$$
=0.2475^2+0.435^2
$$

$$
=0.06125625+0.189225
$$

$$
=\boxed{0.25048125}.
$$

To centre 3:

$$0.$$

Therefore $\mathbf{x}_6\rightarrow C_3$.

No assignment changes. The $K=3$ solution has converged.

Its SSE is

$$
SSE_1=0+0.074314+0.074314=0.148628,
$$

$$
SSE_2=0.02783125+0.02783125=0.0556625,
$$

$$
SSE_3=0.
$$

Therefore

$$
\boxed{SSE_{K=3}=0.148628+0.0556625=0.2042905}.
$$

The reduction from $K=2$ is

$$
0.371278-0.2042905=\boxed{0.1669875}.
$$

Compared with the enormous $11.631390$ reduction from $K=1$ to $K=2$, this is much smaller.

# Complete $K=4$ Run for the Elbow Analysis

Now set

$$K=4.$$

Suppose a random draw selects

$$
\boldsymbol{\mu}_1^{(0)}=\mathbf{x}_1=(-0.991,-0.978),
$$

$$
\boldsymbol{\mu}_2^{(0)}=\mathbf{x}_2=(-0.826,-0.761),
$$

$$
\boldsymbol{\mu}_3^{(0)}=\mathbf{x}_5=(0.991,0.978),
$$

$$
\boldsymbol{\mu}_4^{(0)}=\mathbf{x}_6=(1.156,1.268).
$$

## $K=4$, Iteration 1

### Observation $\mathbf{x}_1$

To centre 1:

$$0.$$

To centre 2:

$$
(-0.165)^2+(-0.217)^2=\boxed{0.074314}.
$$

To centre 3:

$$
(-1.982)^2+(-1.956)^2=\boxed{7.754260}.
$$

To centre 4:

$$
(-2.147)^2+(-2.246)^2=\boxed{9.654125}.
$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Observation $\mathbf{x}_2$

To centre 1:

$$
0.165^2+0.217^2=\boxed{0.074314}.
$$

To centre 2:

$$0.$$

To centre 3:

$$
(-1.817)^2+(-1.739)^2=\boxed{6.325610}.
$$

To centre 4:

$$
(-1.982)^2+(-2.029)^2=\boxed{8.045165}.
$$

Therefore $\mathbf{x}_2\rightarrow C_2$.

### Observation $\mathbf{x}_3$

To centre 1:

$$
(-0.165)^2+(-0.217)^2=\boxed{0.074314}.
$$

To centre 2:

$$
(-0.330)^2+(-0.434)^2=\boxed{0.297256}.
$$

To centre 3:

$$
(-2.147)^2+(-2.173)^2=\boxed{9.331538}.
$$

To centre 4:

$$
(-2.312)^2+(-2.463)^2=\boxed{11.411713}.
$$

Therefore $\mathbf{x}_3\rightarrow C_1$.

### Observation $\mathbf{x}_4$

To centre 1:

$$
1.817^2+1.666^2=\boxed{6.077045}.
$$

To centre 2:

$$
1.652^2+1.449^2
$$

$$
=2.729104+2.099601
$$

$$
=\boxed{4.828705}.
$$

To centre 3:

$$
(-0.165)^2+(-0.290)^2=\boxed{0.111325}.
$$

To centre 4:

$$
(-0.330)^2+(-0.580)^2=\boxed{0.445300}.
$$

Therefore $\mathbf{x}_4\rightarrow C_3$.

### Observation $\mathbf{x}_5$

To centre 1:

$$\boxed{7.754260}.$$

To centre 2:

$$\boxed{6.325610}.$$

To centre 3:

$$0.$$

To centre 4:

$$\boxed{0.111325}.$$

Therefore $\mathbf{x}_5\rightarrow C_3$.

### Observation $\mathbf{x}_6$

To centre 1:

$$\boxed{9.654125}.$$

To centre 2:

$$\boxed{8.045165}.$$

To centre 3:

$$\boxed{0.111325}.$$

To centre 4:

$$0.$$

Therefore $\mathbf{x}_6\rightarrow C_4$.

The first grouping is

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_3\},
$$

$$
C_2=\{\mathbf{x}_2\},
$$

$$
C_3=\{\mathbf{x}_4,\mathbf{x}_5\},
$$

$$
C_4=\{\mathbf{x}_6\}.
$$

## $K=4$, Centre Update

For cluster 1,

$$
\boldsymbol{\mu}_1^{(1)}=\frac{\mathbf{x}_1+\mathbf{x}_3}{2}.
$$

First coordinate:

$$
\frac{-0.991-1.156}{2}=-1.0735.
$$

Second coordinate:

$$
\frac{-0.978-1.195}{2}=-1.0865.
$$

Therefore

$$
\boldsymbol{\mu}_1^{(1)}=(-1.0735,-1.0865).
$$

Cluster 2 is a singleton:

$$
\boldsymbol{\mu}_2^{(1)}=(-0.826,-0.761).
$$

For cluster 3,

$$
\frac{0.826+0.991}{2}=0.9085,
$$

$$
\frac{0.688+0.978}{2}=0.833.$$

Therefore

$$
\boldsymbol{\mu}_3^{(1)}=(0.9085,0.833).
$$

Cluster 4 is a singleton:

$$
\boldsymbol{\mu}_4^{(1)}=(1.156,1.268).
$$

## $K=4$, Iteration 2

### Observation $\mathbf{x}_1$

To centre 1:

$$
0.0825^2+0.1085^2
$$

$$
=0.00680625+0.01177225
$$

$$
=\boxed{0.0185785}.
$$

To centre 2:

$$\boxed{0.074314}.$$

To centre 3:

$$
(-1.8995)^2+(-1.811)^2
$$

$$
=3.60810025+3.279721
$$

$$
=\boxed{6.88782125}.
$$

To centre 4:

$$\boxed{9.654125}.$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Observation $\mathbf{x}_2$

To centre 1:

$$
0.2475^2+0.3255^2
$$

$$
=0.06125625+0.10595025
$$

$$
=\boxed{0.1672065}.
$$

To centre 2:

$$0.$$

To centre 3:

$$
(-1.7345)^2+(-1.594)^2
$$

$$
=3.00849025+2.540836
$$

$$
=\boxed{5.54932625}.
$$

To centre 4:

$$\boxed{8.045165}.$$

Therefore $\mathbf{x}_2\rightarrow C_2$.

### Observation $\mathbf{x}_3$

To centre 1:

$$
(-0.0825)^2+(-0.1085)^2=\boxed{0.0185785}.
$$

To centre 2:

$$\boxed{0.297256}.$$

To centre 3:

$$
(-2.0645)^2+(-2.028)^2
$$

$$
=4.26216025+4.112784
$$

$$
=\boxed{8.37494425}.
$$

To centre 4:

$$\boxed{11.411713}.$$

Therefore $\mathbf{x}_3\rightarrow C_1$.

### Observation $\mathbf{x}_4$

To centre 1:

$$
1.8995^2+1.7745^2
$$

$$
=3.60810025+3.14885025
$$

$$
=\boxed{6.7569505}.
$$

To centre 2:

$$\boxed{4.828705}.$$

To centre 3:

$$
(-0.0825)^2+(-0.145)^2
$$

$$
=0.00680625+0.021025
$$

$$
=\boxed{0.02783125}.
$$

To centre 4:

$$\boxed{0.445300}.$$

Therefore $\mathbf{x}_4\rightarrow C_3$.

### Observation $\mathbf{x}_5$

To centre 1:

$$
2.0645^2+2.0645^2
$$

$$
=4.26216025+4.26216025
$$

$$
=\boxed{8.5243205}.
$$

To centre 2:

$$\boxed{6.325610}.$$

To centre 3:

$$
0.0825^2+0.145^2
$$

$$
=0.00680625+0.021025
$$

$$
=\boxed{0.02783125}.
$$

To centre 4:

$$\boxed{0.111325}.$$

Therefore $\mathbf{x}_5\rightarrow C_3$.

### Observation $\mathbf{x}_6$

To centre 1:

$$
2.2295^2+2.3545^2
$$

$$
=4.97067025+5.54367025
$$

$$
=\boxed{10.5143405}.
$$

To centre 2:

$$\boxed{8.045165}.$$

To centre 3:

$$
0.2475^2+0.435^2
$$

$$
=0.06125625+0.189225
$$

$$
=\boxed{0.25048125}.
$$

To centre 4:

$$0.$$

Therefore $\mathbf{x}_6\rightarrow C_4$.

No assignment changes, so the $K=4$ solution has converged.

The final SSE is

$$
SSE_1=0.0185785+0.0185785=0.037157,
$$

$$SSE_2=0,$$

$$
SSE_3=0.02783125+0.02783125=0.0556625,
$$

$$SSE_4=0.$$

Therefore

$$
\boxed{SSE_{K=4}=0.037157+0.0556625=0.0928195}.
$$

The reduction from $K=3$ is

$$
0.2042905-0.0928195=\boxed{0.111471}.
$$

## Interpreting the Elbow

The results are

\begin{center}
\begin{tabular}{c|c}
\toprule
$K$ & SSE\\
\midrule
1 & 12.002668\\
2 & 0.371278\\
3 & 0.2042905\\
4 & 0.0928195\\
\bottomrule
\end{tabular}
\end{center}

The improvement from $K=1$ to $K=2$ is

$$11.631390,$$

from $K=2$ to $K=3$ it is

$$0.1669875,$$

and from $K=3$ to $K=4$ it is

$$0.111471.$$

The dominant improvement occurs when moving from one cluster to two. After $K=2$, additional clusters continue to reduce SSE, as expected, but by much smaller amounts. Therefore the elbow is around

$$
\boxed{K=2}.
$$

# Silhouette Score

SSE asks how close observations are to their assigned cluster centres. Silhouette asks a different question: **does each observation fit its own cluster better than it fits the nearest alternative cluster?**

For observation $i$ define

$$
a(i)=\text{average distance from }i\text{ to the other observations in its own cluster}
$$

and

$$
b(i)=\text{smallest average distance from }i\text{ to another cluster}.
$$

The silhouette score is

$$
\boxed{
s(i)=\frac{b(i)-a(i)}{\max(a(i),b(i))}
}.
$$

Interpretation:

$$s(i)\approx1$$

means the observation is strongly associated with its own cluster;

$$s(i)\approx0$$

means the observation lies near a cluster boundary; and

$$s(i)<0$$

suggests that another cluster may fit the observation better.

For the worked silhouette calculations we use ordinary Euclidean distance, not squared Euclidean distance.

# Pairwise Distances for Silhouette and Dunn Calculations

The unique point-to-point distances are calculated from

$$
d(\mathbf{x}_i,\mathbf{x}_j)=\sqrt{\|\mathbf{x}_i-\mathbf{x}_j\|^2}.
$$

For example,

$$
d(\mathbf{x}_1,\mathbf{x}_2)=\sqrt{0.074314}=\boxed{0.272606}.
$$

The full set required later is

$$d_{12}=0.272606,$$

$$d_{13}=0.272606,$$

$$d_{14}=\sqrt{6.077045}=2.465166,$$

$$d_{15}=\sqrt{7.754260}=2.784647,$$

$$d_{16}=\sqrt{9.654125}=3.107109,$$

$$d_{23}=\sqrt{0.297256}=0.545212,$$

$$d_{24}=\sqrt{4.828705}=2.197431,$$

$$d_{25}=\sqrt{6.325610}=2.515077,$$

$$d_{26}=\sqrt{8.045165}=2.836400,$$

$$d_{34}=\sqrt{7.474013}=2.733864,$$

$$d_{35}=\sqrt{9.331538}=3.054757,$$

$$d_{36}=\sqrt{11.411713}=3.378123,$$

$$d_{45}=\sqrt{0.111325}=0.333654,$$

$$d_{46}=\sqrt{0.445300}=0.667308,$$

$$d_{56}=\sqrt{0.111325}=0.333654.$$

# Silhouette Score for $K=2$

The converged $K=2$ clusters are

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\},
\qquad
C_2=\{\mathbf{x}_4,\mathbf{x}_5,\mathbf{x}_6\}.
$$

## Observation $\mathbf{x}_1$

Its within-cluster distances are

$$d(\mathbf{x}_1,\mathbf{x}_2)=0.272606,$$

$$d(\mathbf{x}_1,\mathbf{x}_3)=0.272606.$$

Therefore

$$
a(1)=\frac{0.272606+0.272606}{2}=\boxed{0.272606}.
$$

Distances to the other cluster are

$$d(\mathbf{x}_1,\mathbf{x}_4)=2.465166,$$

$$d(\mathbf{x}_1,\mathbf{x}_5)=2.784647,$$

$$d(\mathbf{x}_1,\mathbf{x}_6)=3.107109.$$

Therefore

$$
b(1)=\frac{2.465166+2.784647+3.107109}{3}.
$$

Add:

$$2.465166+2.784647=5.249813,$$

$$5.249813+3.107109=8.356922.$$

Hence

$$
b(1)=\frac{8.356922}{3}=\boxed{2.785641}.
$$

Now

$$
s(1)=\frac{2.785641-0.272606}{2.785641}
$$

$$
=\frac{2.513035}{2.785641}
$$

$$
=\boxed{0.902\text{ approximately}}.
$$

## Observation $\mathbf{x}_2$

Within its own cluster,

$$d(\mathbf{x}_2,\mathbf{x}_1)=0.272606,$$

$$d(\mathbf{x}_2,\mathbf{x}_3)=0.545212.$$

Therefore

$$
a(2)=\frac{0.272606+0.545212}{2}
$$

$$
=\frac{0.817818}{2}
$$

$$
=\boxed{0.408909}.
$$

Distances to $C_2$ are

$$2.197431,\quad2.515077,\quad2.836400.$$

Therefore

$$
b(2)=\frac{2.197431+2.515077+2.836400}{3}.
$$

Add:

$$2.197431+2.515077=4.712508,$$

$$4.712508+2.836400=7.548908.$$

Thus

$$
b(2)=\frac{7.548908}{3}=\boxed{2.516303}.
$$

The silhouette is

$$
s(2)=\frac{2.516303-0.408909}{2.516303}
$$

$$
=\frac{2.107394}{2.516303}
$$

$$
=\boxed{0.837\text{ approximately}}.
$$

## Observation $\mathbf{x}_3$

Within-cluster distances are

$$0.272606\quad\text{and}\quad0.545212.$$

Therefore

$$
a(3)=\frac{0.272606+0.545212}{2}=\boxed{0.408909}.
$$

Distances to the other cluster are

$$2.733864,\quad3.054757,\quad3.378123.$$

Therefore

$$
b(3)=\frac{2.733864+3.054757+3.378123}{3}.
$$

Add:

$$2.733864+3.054757=5.788621,$$

$$5.788621+3.378123=9.166744.$$

Hence

$$
b(3)=\frac{9.166744}{3}=\boxed{3.055581}.
$$

Then

$$
s(3)=\frac{3.055581-0.408909}{3.055581}
$$

$$
=\frac{2.646672}{3.055581}
$$

$$
=\boxed{0.866\text{ approximately}}.
$$

## Observation $\mathbf{x}_4$

Within $C_2$,

$$d(\mathbf{x}_4,\mathbf{x}_5)=0.333654,$$

$$d(\mathbf{x}_4,\mathbf{x}_6)=0.667308.$$

Therefore

$$
a(4)=\frac{0.333654+0.667308}{2}
$$

$$
=\frac{1.000962}{2}
$$

$$
=\boxed{0.500481}.
$$

Distances to $C_1$ are

$$2.465166,\quad2.197431,\quad2.733864.$$

Therefore

$$
b(4)=\frac{2.465166+2.197431+2.733864}{3}.
$$

Add:

$$2.465166+2.197431=4.662597,$$

$$4.662597+2.733864=7.396461.$$

Thus

$$
b(4)=\frac{7.396461}{3}=\boxed{2.465487}.
$$

Then

$$
s(4)=\frac{2.465487-0.500481}{2.465487}
$$

$$
=\frac{1.965006}{2.465487}
$$

$$
=\boxed{0.797\text{ approximately}}.
$$

## Observation $\mathbf{x}_5$

Within-cluster distances are

$$0.333654\quad\text{and}\quad0.333654.$$

Therefore

$$
a(5)=\frac{0.333654+0.333654}{2}=\boxed{0.333654}.
$$

Distances to $C_1$ are

$$2.784647,\quad2.515077,\quad3.054757.$$

Therefore

$$
b(5)=\frac{2.784647+2.515077+3.054757}{3}.
$$

Add:

$$2.784647+2.515077=5.299724,$$

$$5.299724+3.054757=8.354481.$$

Hence

$$
b(5)=\frac{8.354481}{3}=\boxed{2.784827}.
$$

Then

$$
s(5)=\frac{2.784827-0.333654}{2.784827}
$$

$$
=\frac{2.451173}{2.784827}
$$

$$
=\boxed{0.880\text{ approximately}}.
$$

## Observation $\mathbf{x}_6$

Within-cluster distances are

$$0.667308\quad\text{and}\quad0.333654.$$

Therefore

$$
a(6)=\frac{0.667308+0.333654}{2}
$$

$$
=\frac{1.000962}{2}
$$

$$
=\boxed{0.500481}.
$$

Distances to $C_1$ are

$$3.107109,\quad2.836400,\quad3.378123.$$

Therefore

$$
b(6)=\frac{3.107109+2.836400+3.378123}{3}.
$$

Add:

$$3.107109+2.836400=5.943509,$$

$$5.943509+3.378123=9.321632.$$

Hence

$$
b(6)=\frac{9.321632}{3}=\boxed{3.107211}.
$$

Then

$$
s(6)=\frac{3.107211-0.500481}{3.107211}
$$

$$
=\frac{2.606730}{3.107211}
$$

$$
=\boxed{0.839\text{ approximately}}.
$$

## Average Silhouette for $K=2$

Add all six scores:

$$
0.902+0.837=1.739,
$$

$$
1.739+0.866=2.605,
$$

$$
2.605+0.797=3.402,
$$

$$
3.402+0.880=4.282,
$$

$$
4.282+0.839=5.121.
$$

Therefore

$$
\bar{s}_{K=2}=\frac{5.121}{6}=\boxed{0.854\text{ approximately}}.
$$

# Silhouette Score for $K=3$

The $K=3$ clusters are

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\},
$$

$$
C_2=\{\mathbf{x}_4,\mathbf{x}_5\},
$$

$$
C_3=\{\mathbf{x}_6\}.
$$

With more than two clusters, $b(i)$ is the **smallest** average distance to any alternative cluster.

## Observation $\mathbf{x}_1$

Its within-cluster value remains

$$a(1)=0.272606.$$

Average distance to $C_2$:

$$
\frac{2.465166+2.784647}{2}
$$

$$
=\frac{5.249813}{2}
$$

$$
=\boxed{2.624907}.
$$

Average distance to $C_3$ is simply the distance to $\mathbf{x}_6$:

$$3.107109.$$

Therefore

$$
b(1)=\min(2.624907,3.107109)=2.624907.
$$

Hence

$$
s(1)=\frac{2.624907-0.272606}{2.624907}
$$

$$
=\frac{2.352301}{2.624907}
$$

$$
=\boxed{0.896\text{ approximately}}.
$$

## Observation $\mathbf{x}_2$

$$a(2)=0.408909.$$

Average distance to $C_2$:

$$
\frac{2.197431+2.515077}{2}
$$

$$
=\frac{4.712508}{2}
$$

$$
=\boxed{2.356254}.
$$

Average distance to $C_3$:

$$2.836400.$$

Thus

$$
b(2)=2.356254.
$$

Then

$$
s(2)=\frac{2.356254-0.408909}{2.356254}
$$

$$
=\frac{1.947345}{2.356254}
$$

$$
=\boxed{0.826\text{ approximately}}.
$$

## Observation $\mathbf{x}_3$

$$a(3)=0.408909.$$

Average distance to $C_2$:

$$
\frac{2.733864+3.054757}{2}
$$

$$
=\frac{5.788621}{2}
$$

$$
=\boxed{2.894311}.
$$

Average distance to $C_3$:

$$3.378123.$$

Therefore

$$b(3)=2.894311.$$

Hence

$$
s(3)=\frac{2.894311-0.408909}{2.894311}
$$

$$
=\frac{2.485402}{2.894311}
$$

$$
=\boxed{0.859\text{ approximately}}.
$$

## Observation $\mathbf{x}_4$

Cluster $C_2$ contains only $\mathbf{x}_4$ and $\mathbf{x}_5$, so

$$
a(4)=d(\mathbf{x}_4,\mathbf{x}_5)=\boxed{0.333654}.
$$

Average distance to $C_1$:

$$
\frac{2.465166+2.197431+2.733864}{3}
$$

$$
=\frac{7.396461}{3}
$$

$$
=\boxed{2.465487}.
$$

Distance to $C_3$:

$$d(\mathbf{x}_4,\mathbf{x}_6)=0.667308.$$

Therefore

$$
b(4)=\min(2.465487,0.667308)=0.667308.
$$

Hence

$$
s(4)=\frac{0.667308-0.333654}{0.667308}
$$

$$
=\frac{0.333654}{0.667308}
$$

$$
=\boxed{0.5}.
$$

## Observation $\mathbf{x}_5$

$$a(5)=d(\mathbf{x}_5,\mathbf{x}_4)=0.333654.$$

Average distance to $C_1$:

$$
\frac{2.784647+2.515077+3.054757}{3}
=2.784827.
$$

Distance to $C_3$:

$$d(\mathbf{x}_5,\mathbf{x}_6)=0.333654.$$

Thus

$$b(5)=0.333654.$$

Therefore

$$
s(5)=\frac{0.333654-0.333654}{0.333654}=\boxed{0}.
$$

This observation is effectively on the boundary between its own cluster and the nearest alternative cluster.

## Observation $\mathbf{x}_6$: Singleton Cluster

$\mathbf{x}_6$ is the only observation in $C_3$. There is no second observation in the same cluster from which to calculate an ordinary within-cluster average distance. Using the standard silhouette convention for singleton clusters,

$$
\boxed{s(6)=0}.
$$

## Average Silhouette for $K=3$

Add:

$$0.896+0.826=1.722,$$

$$1.722+0.859=2.581,$$

$$2.581+0.500=3.081,$$

and the last two scores add zero. Hence

$$
\bar{s}_{K=3}=\frac{3.081}{6}=\boxed{0.514\text{ approximately}}.
$$
Compare:

$$
\bar{s}_{K=2}\approx0.854,
\qquad
\bar{s}_{K=3}\approx0.514.
$$

Because higher silhouette is better,

$$
\boxed{\text{silhouette strongly prefers }K=2}.
$$

# Dunn's Index

A common form of Dunn's index is

$$
\boxed{
D=\frac{\text{minimum distance between two different clusters}}{\text{maximum diameter of any cluster}}
}.
$$

The numerator rewards separation between clusters. The denominator penalizes large spread inside a cluster. Therefore a larger Dunn index is desirable.

## Dunn Index for $K=2$

For

$$C_1=\{\mathbf{x}_1,\mathbf{x}_2,\mathbf{x}_3\},$$

within-cluster distances are

$$0.272606,\quad0.272606,\quad0.545212.$$

Therefore

$$
\operatorname{diam}(C_1)=\boxed{0.545212}.
$$

For

$$C_2=\{\mathbf{x}_4,\mathbf{x}_5,\mathbf{x}_6\},$$

within-cluster distances are

$$0.333654,\quad0.667308,\quad0.333654.$$

Therefore

$$
\operatorname{diam}(C_2)=\boxed{0.667308}.
$$

The maximum within-cluster diameter is

$$
\boxed{0.667308}.
$$

Now inspect every cross-cluster distance:

$$d_{14}=2.465166,$$

$$d_{15}=2.784647,$$

$$d_{16}=3.107109,$$

$$d_{24}=2.197431,$$

$$d_{25}=2.515077,$$

$$d_{26}=2.836400,$$

$$d_{34}=2.733864,$$

$$d_{35}=3.054757,$$

$$d_{36}=3.378123.$$

The smallest is

$$
\boxed{2.197431},
$$

between $\mathbf{x}_2$ and $\mathbf{x}_4$.

Therefore

$$
D_{K=2}=\frac{2.197431}{0.667308}
$$

$$
=\boxed{3.293\text{ approximately}}.
$$

## Dunn Index for $K=3$

The cluster diameters are

$$
\operatorname{diam}(C_1)=0.545212,
$$

$$
\operatorname{diam}(C_2)=0.333654,
$$

$$
\operatorname{diam}(C_3)=0
$$

because $C_3$ is a singleton. The largest diameter is therefore

$$
\boxed{0.545212}.
$$

Minimum separation between $C_1$ and $C_2$:

$$
\boxed{2.197431}.
$$

Between $C_1$ and $C_3$, the relevant distances are

$$3.107109,\quad2.836400,\quad3.378123,$$

so the minimum is

$$
\boxed{2.836400}.
$$

Between $C_2$ and $C_3$:

$$d(\mathbf{x}_4,\mathbf{x}_6)=0.667308,$$

$$d(\mathbf{x}_5,\mathbf{x}_6)=0.333654,$$

so the minimum is

$$
\boxed{0.333654}.
$$

The smallest separation across all cluster pairs is therefore

$$
0.333654.
$$

Thus

$$
D_{K=3}=\frac{0.333654}{0.545212}
$$

$$
=\boxed{0.612\text{ approximately}}.
$$

Compare:

$$3.293>0.612.$$

Therefore

$$
\boxed{\text{Dunn's index prefers }K=2}.
$$

# Combined Evidence for Choosing $K$

For this teaching dataset, all three evaluation approaches agree:

\begin{center}
\begin{tabular}{l|l}
\toprule
Method & Result\\
\midrule
SSE / elbow & $K\approx2$\\
Average silhouette & $K=2$\\
Dunn's index & $K=2$\\
\bottomrule
\end{tabular}
\end{center}

A defensible conclusion is therefore:

> For this dataset, $K=2$ is supported by the sharp elbow in SSE, the higher average silhouette score, and the larger Dunn index.

# Which $K$-Selection Method Is Fastest and Easiest?

Among the methods studied here, the quickest and simplest practical first check is usually the **elbow method**. Run K-means for several candidate values of $K$, record SSE, and look for the point where the reduction changes from large to relatively small.

Its weakness is that some datasets do not produce a clear visual elbow. Silhouette score often gives stronger information about how well individual observations fit their clusters, but requires additional pairwise-distance calculations. Dunn's index also evaluates compactness and separation, but is generally less convenient to calculate manually.

A useful workflow is therefore

$$
\boxed{\text{elbow for a quick estimate}\;\longrightarrow\;\text{silhouette/Dunn for supporting evidence}}.
$$

# Must Data Always Be Standardised?

No.

$$
\boxed{\text{K-means data do not always have to be standardised}}.
$$

Standardisation is particularly important when features use different units or have very different numerical scales, because Euclidean distance can otherwise be dominated by the larger-scale feature.

For example, suppose two features contribute differences of 1 and 20. The squared distance includes

$$
1^2+20^2=1+400=401.
$$

The second feature contributes 400 while the first contributes only 1. If that dominance is merely a consequence of units or scale rather than genuine importance, standardisation is appropriate.

If all features are already naturally comparable in scale and meaning, standardisation may not be necessary. Bishop's RGB image example, for instance, applies K-means directly to the three comparable colour-channel intensities.

# Important Limitations of K-Means

## Sensitivity to Outliers

K-means centres are arithmetic means, so extreme observations can pull a centre away from the majority of the cluster.

Suppose a cluster contains

$$52,\quad55,\quad58.$$

Its mean is

$$
\mu=\frac{52+55+58}{3}
=\frac{165}{3}
=\boxed{55}.
$$

Now add one extreme observation, 120. The new mean is

$$
\mu=\frac{52+55+58+120}{4}.
$$

Add:

$$52+55=107,$$

$$107+58=165,$$

$$165+120=285.$$

Therefore

$$
\mu=\frac{285}{4}=\boxed{71.25}.
$$

One observation has shifted the centre from 55 to 71.25, even though three of the four observations are still between 52 and 58.

The squared-distance objective also makes very large deviations influential. For the outlier,

$$
120-71.25=48.75,
$$

so

$$
48.75^2=\boxed{2376.5625}.
$$

For the point 58,

$$
58-71.25=-13.25,
$$

so

$$
(-13.25)^2=\boxed{175.5625}.
$$

Hence K-means can be strongly influenced by outliers.

## Categorical Variables

Suppose a feature is

$$
\text{Material}\in\{\text{Steel, Aluminium, Titanium}\}.
$$

Ordinary K-means requires operations such as subtraction, Euclidean distance, and arithmetic means. But an expression such as

$$
\text{Steel}-\text{Titanium}
$$

has no numerical meaning.

One might encode the categories as

$$
\text{Steel}=1,\quad\text{Aluminium}=2,\quad\text{Titanium}=3.
$$

Then K-means would interpret

$$|1-2|=1$$

and

$$|1-3|=2,$$

which says Steel is twice as far from Titanium as from Aluminium. That geometry was introduced entirely by the arbitrary coding scheme. A different numeric coding would create a different geometry.

Therefore ordinary K-means is designed for numerical variables for which Euclidean distances and arithmetic means are meaningful.

## Hard Assignment

K-means uses

$$r_{nk}\in\{0,1\}.$$

Suppose two one-dimensional centres are

$$
\mu_1=0,\qquad\mu_2=10.
$$

For

$$x=4.9,$$

the distances are

$$|4.9-0|=4.9,$$

$$|4.9-10|=5.1.$$

Therefore K-means assigns the point fully to cluster 1:

$$
r_{n1}=1,\qquad r_{n2}=0.
$$

Now move the point only 0.2 units to

$$x=5.1.$$

The distances become

$$|5.1-0|=5.1,$$

$$|5.1-10|=4.9.$$

The assignment completely flips:

$$
r_{n1}=0,\qquad r_{n2}=1.
$$

The point is almost midway between the two centres, yet K-means makes an absolute 0-or-1 decision. Bishop uses this limitation to motivate probabilistic **soft assignments** in Gaussian mixture models.

## Computational Cost

During each assignment step, every observation is compared with every cluster centre. With $N$ observations and $K$ centres, this requires work of approximately

$$
\boxed{O(KN)}
$$

per assignment pass.

For the small example with

$$N=6,\qquad K=4,$$

we perform

$$6\times4=24$$

point-centre comparisons in one assignment pass.

For

$$N=10,000,000$$

and

$$K=100,$$

one pass involves approximately

$$
100\times10,000,000=\boxed{1,000,000,000}
$$

point-centre comparisons. Thus a direct implementation can become expensive on very large datasets.

# K-Medoids as a Generalisation

Bishop generalises the squared Euclidean dissimilarity by introducing a more general function

$$
V(\mathbf{x},\mathbf{x}').
$$

The corresponding distortion measure is

$$
\widetilde{J}=
\sum_{n=1}^{N}
\sum_{k=1}^{K}
r_{nk}V(\mathbf{x}_n,\boldsymbol{\mu}_k).
$$

This leads to K-medoids. A practical distinction is that K-means represents a cluster by its arithmetic mean, whereas K-medoids can restrict the representative to an actual observation and permit more general dissimilarity measures.

For the present course sequence, the important distinction is simply

$$
\boxed{\text{K-means: mean + squared Euclidean distance}}
$$

versus

$$
\boxed{\text{K-medoids: representative data point + more general dissimilarity}}.
$$

# Image Segmentation with K-Means

Bishop illustrates K-means using image segmentation and image compression. A colour pixel can be represented as a three-dimensional vector

$$
\boxed{
\mathbf{x}_n=
\begin{bmatrix}
R\\G\\B
\end{bmatrix}
}.
$$

Thus a pixel is simply another observation. Instead of geyser features, its three features are red, green, and blue intensity.

For a chosen $K$, K-means clusters pixels according to colour similarity. After convergence, each pixel can be redrawn using the RGB value of the centre to which it was assigned. The image is therefore represented using a palette of only $K$ colours.

A limitation is that this simple version uses colour only. It does not know where pixels are located in the image. Two similarly coloured pixels on opposite sides of an image can be placed in the same cluster even if they belong to different objects.

## Small RGB Teaching Example

Consider four pixels

$$
\mathbf{x}_1=
\begin{bmatrix}250\\20\\20\end{bmatrix},
\qquad
\mathbf{x}_2=
\begin{bmatrix}240\\30\\25\end{bmatrix},
$$

$$
\mathbf{x}_3=
\begin{bmatrix}20\\30\\240\end{bmatrix},
\qquad
\mathbf{x}_4=
\begin{bmatrix}30\\20\\250\end{bmatrix}.
$$

Choose

$$K=2.$$

Suppose the initial centres are

$$
\boldsymbol{\mu}_1^{(0)}=
\begin{bmatrix}250\\20\\20\end{bmatrix}
$$

and

$$
\boldsymbol{\mu}_2^{(0)}=
\begin{bmatrix}20\\30\\240\end{bmatrix}.
$$

### Pixel $\mathbf{x}_1$

To centre 1:

$$
(250-250)^2+(20-20)^2+(20-20)^2=\boxed{0}.
$$

To centre 2:

$$
(250-20)^2+(20-30)^2+(20-240)^2
$$

$$
=230^2+(-10)^2+(-220)^2
$$

$$
=52900+100+48400
$$

$$
=\boxed{101400}.
$$

Therefore $\mathbf{x}_1\rightarrow C_1$.

### Pixel $\mathbf{x}_2$

To centre 1:

$$
(240-250)^2+(30-20)^2+(25-20)^2
$$

$$
=(-10)^2+10^2+5^2
$$

$$
=100+100+25
$$

$$
=\boxed{225}.
$$

To centre 2:

$$
(240-20)^2+(30-30)^2+(25-240)^2
$$

$$
=220^2+0^2+(-215)^2
$$

$$
=48400+0+46225
$$

$$
=\boxed{94625}.
$$

Therefore $\mathbf{x}_2\rightarrow C_1$.

### Pixel $\mathbf{x}_3$

To centre 1:

$$
(20-250)^2+(30-20)^2+(240-20)^2
$$

$$
=(-230)^2+10^2+220^2
$$

$$
=52900+100+48400
$$

$$
=\boxed{101400}.
$$

To centre 2:

$$
(20-20)^2+(30-30)^2+(240-240)^2=\boxed{0}.
$$

Therefore $\mathbf{x}_3\rightarrow C_2$.

### Pixel $\mathbf{x}_4$

To centre 1:

$$
(30-250)^2+(20-20)^2+(250-20)^2
$$

$$
=(-220)^2+0^2+230^2
$$

$$
=48400+0+52900
$$

$$
=\boxed{101300}.
$$

To centre 2:

$$
(30-20)^2+(20-30)^2+(250-240)^2
$$

$$
=10^2+(-10)^2+10^2
$$

$$
=100+100+100
$$

$$
=\boxed{300}.
$$

Therefore $\mathbf{x}_4\rightarrow C_2$.

So

$$
C_1=\{\mathbf{x}_1,\mathbf{x}_2\},
\qquad
C_2=\{\mathbf{x}_3,\mathbf{x}_4\}.
$$

Update centre 1:

$$
\boldsymbol{\mu}_1=\frac{\mathbf{x}_1+\mathbf{x}_2}{2}.
$$

Red component:

$$
\frac{250+240}{2}=245.
$$

Green component:

$$
\frac{20+30}{2}=25.
$$

Blue component:

$$
\frac{20+25}{2}=22.5.
$$

Therefore

$$
\boxed{\boldsymbol{\mu}_1=(245,25,22.5)}.
$$

Update centre 2:

$$
\frac{20+30}{2}=25,
$$

$$
\frac{30+20}{2}=25,
$$

$$
\frac{240+250}{2}=245.
$$

Therefore

$$
\boxed{\boldsymbol{\mu}_2=(25,25,245)}.
$$

The first two pixels can now be approximated by one reddish representative colour, and the last two by one bluish representative colour.

# Image Compression and Vector Quantisation

K-means clustering can also be used for **lossy data compression**. Instead of storing every original vector, store only the index of the nearest cluster centre, together with the $K$ centre vectors themselves. Bishop calls this framework **vector quantisation**, and the centre vectors are called **code-book vectors**.

Suppose each RGB channel uses 8 bits. A complete RGB pixel therefore requires

$$
8+8+8=\boxed{24\text{ bits}}.
$$

For an image with $N$ pixels, the uncompressed storage is

$$
\boxed{24N\text{ bits}}.
$$

After K-means, each pixel is represented by the identity of one of $K$ cluster centres. The number of bits needed for that label is

$$
\lceil\log_2K\rceil.
$$

The $K$ RGB centre vectors themselves require

$$24K$$

bits. Therefore the simplified compressed size is

$$
\boxed{
24K+N\lceil\log_2K\rceil
}.
$$

## Why $\log_2K$ Bits?

For $K=2$, there are two labels. One bit has two states, 0 and 1, so

$$
\log_2 2=1.
$$

For $K=4$, four binary codes are available with two bits:

$$00,\quad01,\quad10,\quad11,$$

and

$$
\log_2 4=2.
$$

For $K=8$,

$$
\log_2 8=3.
$$

For $K=3$,

$$
\log_2 3\approx1.585,
$$

so a whole-number representation requires 2 bits.

# Bishop's Numerical Image Compression Example

The image contains

$$240\times180$$

pixels. Therefore

$$
N=240\times180=\boxed{43,200}.
$$

Without compression, the image requires

$$
24N=24(43,200).
$$

Calculate:

$$43,200\times20=864,000,$$

$$43,200\times4=172,800.$$

Therefore

$$
864,000+172,800=\boxed{1,036,800\text{ bits}}.
$$

## Compression with $K=2$

Because

$$\log_2 2=1,$$

pixel labels require

$$
43,200(1)=43,200\text{ bits}.
$$

The two RGB code-book vectors require

$$
24(2)=48\text{ bits}.
$$

Total:

$$
43,200+48=\boxed{43,248\text{ bits}}.
$$

Relative to the original,

$$
\frac{43,248}{1,036,800}\times100\approx\boxed{4.2\%}.
$$

## Compression with $K=3$

Because

$$\log_2 3\approx1.585,$$

we use 2 bits per pixel label.

Pixel labels require

$$
43,200(2)=86,400\text{ bits}.
$$

Three code-book vectors require

$$
24(3)=72\text{ bits}.
$$

Total:

$$
86,400+72=\boxed{86,472\text{ bits}}.
$$

Relative to the original,

$$
\frac{86,472}{1,036,800}\times100\approx\boxed{8.3\%}.
$$

## Compression with $K=10$

Because

$$\log_2 10\approx3.322,$$

we need 4 bits per pixel label.

Pixel labels require

$$
43,200(4)=172,800\text{ bits}.
$$

Ten code-book vectors require

$$
24(10)=240\text{ bits}.
$$

Total:

$$
172,800+240=\boxed{173,040\text{ bits}}.
$$

Relative to the original,

$$
\frac{173,040}{1,036,800}\times100\approx\boxed{16.7\%}.
$$

The three results are

\begin{center}
\begin{tabular}{c|r|c}
\toprule
$K$ & Compressed bits & Size relative to original\\
\midrule
2 & 43,248 & 4.2\%\\
3 & 86,472 & 8.3\%\\
10 & 173,040 & 16.7\%\\
\bottomrule
\end{tabular}
\end{center}

Therefore

$$
\boxed{\text{smaller }K\Rightarrow\text{greater compression but poorer reconstruction}}
$$

and

$$
\boxed{\text{larger }K\Rightarrow\text{better reconstruction but less compression}}.
$$

The procedure is **lossy** because the reconstructed pixel is generally a cluster centre rather than its exact original RGB value.

# Complete K-Means Algorithm

The complete batch K-means procedure studied here is:

1. Choose $K$, the number of clusters.
2. Choose $K$ initial cluster centres, for example a random subset of observations.
3. For every observation, calculate its squared Euclidean distance to every centre.
4. Assign each observation to its nearest centre.
5. Recalculate every centre as the mean of the observations assigned to that cluster.
6. Repeat the assignment and centre-update steps.
7. Stop when assignments no longer change, or when a specified maximum iteration count is reached.
8. Evaluate the final objective

$$
J=
\sum_{n=1}^{N}
\sum_{k=1}^{K}
r_{nk}\|\mathbf{x}_n-\boldsymbol{\mu}_k\|^2.
$$

# Final Conceptual Summary

K-means seeks groups in which observations are relatively close to others in the same cluster and relatively far from observations outside that cluster. It alternates between two central operations:

$$
\boxed{\text{nearest-centre assignment}}
$$

and

$$
\boxed{\text{centre = mean of assigned observations}}.
$$

The mean is not arbitrary. It follows mathematically from minimising the sum of squared Euclidean distances for a fixed set of assignments.

The main practical lessons are:

- $K$ is the number of clusters and therefore the number of cluster centres.
- Each $\mathbf{x}_n$ is one observation; its components are features.
- Standardisation is useful when feature scales or units are not comparable, but it is not mandatory for every dataset.
- Initialization matters because K-means can converge to different local minima.
- Convergence means the iterative solution has settled, normally because assignments no longer change.
- Convergence does not guarantee the global minimum.
- SSE decreases as $K$ increases, so SSE alone cannot select $K$.
- The elbow method is generally the fastest and simplest first estimate of $K$.
- Silhouette score and Dunn's index provide additional evidence about cluster quality.
- Outliers can pull cluster means.
- Arbitrarily coded categorical variables do not naturally support Euclidean distance and arithmetic means.
- K-means uses hard 0/1 assignments and therefore does not express uncertainty about borderline observations.
- The assignment stage costs approximately $O(KN)$ distance evaluations per iteration.
- K-means can be applied to RGB pixel vectors for simple image segmentation and lossy compression through vector quantisation.

The hard-assignment limitation is the natural bridge to the next course topic:

$$
\boxed{\text{Gaussian Mixture Models (GMMs)}}.
$$

K-means says, in effect, "this observation belongs to exactly one cluster." GMMs replace that hard decision with probabilistic or **soft** membership.

# Primary Source Notes

- Christopher M. Bishop, *Pattern Recognition and Machine Learning*, Chapter 9, especially Section 9.1, **K-means Clustering**, and Section 9.1.1, **Image segmentation and compression**.
- Supplied CSCM445 Machine Learning course material identifying K-means as an unsupervised learning topic and listing SSE, silhouette score, Dunn's index, and the elbow method in the discussion of selecting the number of clusters.
- All small numeric examples introduced specifically for explanation in this tutorial are marked as teaching examples and are not represented as exact data copied from Bishop.