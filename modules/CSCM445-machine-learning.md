# CSCM445 — Machine Learning

## Official syllabus

1. Introduction to machine learning and AI
2. K-means clustering
3. Gaussian Mixture Models (GMMs)
4. Linear Regression
5. Principal Component Analysis (PCA)
6. Linear Discriminant Analysis (LDA)
7. Neural Networks (NN)
8. Convolutional Neural Networks (CNNs)
9. Support Vector Machine (SVM)
10. Logistic Regression (LR)
11. Revision

## Foundation bridge — tailored, not additional syllabus

Open only as needed for the active topic:

- vectors, matrices, dot products and matrix multiplication;
- mean, variance, covariance and Gaussian distributions;
- derivatives, partial derivatives and gradients;
- optimisation intuition: objective/loss functions and minima;
- basic Python/NumPy array manipulation;
- train/validation/test distinction and common evaluation concepts.

## First-principles path by official topic

### 1. Introduction to ML and AI

Start from function approximation and pattern discovery: data, inputs/features, targets, model, loss, learning, generalisation. Distinguish supervised/unsupervised/reinforcement learning because the supplied learning outcomes explicitly require all three concepts.

### 2. K-means

Distance -> within-cluster variance objective -> assignment step -> centroid update -> convergence -> initialization and local minima -> implementation.

### 3. GMM

Gaussian density -> mixture model -> latent cluster membership -> likelihood -> responsibility -> EM intuition/derivation -> comparison with K-means.

### 4. Linear regression

Line/plane model -> residual -> least squares -> matrix formulation -> normal equations / numerical solution -> evaluation in Python.

### 5. PCA

Centering -> variance/covariance -> directions of maximum variance -> eigenvectors/eigenvalues or SVD -> projection -> explained variance.

### 6. LDA

Class separation -> within-class vs between-class scatter -> discriminant direction -> projection/classification -> contrast with PCA.

### 7. Neural networks

Linear model -> nonlinear activation -> composition -> loss -> chain rule -> backpropagation -> optimisation -> evaluation.

### 8. CNNs

Spatial structure -> local receptive fields -> convolution -> kernels/features -> pooling/stride -> stacked feature hierarchy -> training/evaluation.

### 9. SVM

Separating hyperplane -> margin -> constrained optimisation intuition -> support vectors -> soft margin -> kernels only after linear case is secure.

### 10. Logistic regression

Probability of a binary class -> odds/log-odds -> sigmoid -> likelihood / cross-entropy -> decision boundary -> evaluation.

### 11. Revision

Interleave method selection, assumptions, objective functions, derivations, implementation and interpretation rather than rereading notes sequentially.

## Implementation policy

Use Python for ML exercises unless the university task specifies otherwise. Libraries may verify results only after the underlying calculation/algorithm is understood at the appropriate level.
