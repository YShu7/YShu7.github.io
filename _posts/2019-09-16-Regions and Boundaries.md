---
title: Regions & Boundaries
tags: [Computer Vision]
mathjax: true
---

### Boundaries

##### Why extract edges?

Resilient to lighting and color -> useful for recognition

Cue for shape and geometry -> useful for recognition, understanding 3D

##### What are image edges?

Very sharp discontinuities in the **intensity**

##### How to detect edges?

Edges correspond to extrema of derivative. Since images are discrete, we need to take finite differences.

### Filters

#### The Sobel Filter

-  **Horizontal** sobel filter

Horizontal gradients -> vertical lines

$\begin{bmatrix} 1 & 0 & -1 \\\ 2 & 0 & -2 \\\ 1 & 0 & -1 \end{bmatrix} = \begin{bmatrix} 1 \\\ 2 \\\ 1 \end{bmatrix} * \begin{bmatrix} 1 & 0 & -1\end{bmatrix}$

$\begin{bmatrix} 1 \\\ 2 \\\ 1 \end{bmatrix}$ is bulrring matrix

$\begin{bmatrix} 1 & 0 & -1\end{bmatrix}$ is 1D derivative filter

-  **Vertical** sobel filter

Vertical gradients -> horizontal lines

$\begin{bmatrix} 1 & 2 & 1 \\\ 0 & 0 & 0 \\\ -1 & -2 & -1 \end{bmatrix} = \begin{bmatrix} 1 \\\ 0 \\\ -1 \end{bmatrix} * \begin{bmatrix} 1 & 2 & 1\end{bmatrix}$

#### Computing Image Gradients

1. Select derivative filters.

2. Convolve with the image to comput derivatives

   $\frac{\partial f}{\partial x} = S_x \otimes f$

   $\frac{\partial f}{\partial y} = S_y \otimes f$

3. Form the image gradient, and compute its direction and magnitude.

   gradient: $\bigtriangledown f = [\frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}]$

   ​	A vector that points in the direction of most rapid change in **intensity**

   gradient direction: $\theta = tan^{-1} (\frac{\partial f}{\partial y}/\frac{\partial f}{\partial x})$

   gradient magnitude: $\|\bigtriangledown f\| = \sqrt{(\frac{\partial f}{\partial x})^2 + (\frac{\partial f}{\partial y})^2}$

   ​	Edge strength

#### Effects of Noise

- Noise is high frequency
- Differentiation accentuates noise, very sensitive to noise

**Must blur first before using derivative filters**, then look for peaks in $\frac {\partial}{\partial x} (h \otimes f)$

However, smoothing removes the noise, but also blurs the edge.

$\because \frac {\partial}{\partial x} (h \otimes f) =\frac {\partial}{\partial x} (h) \otimes f$ by differentiation property of convolution, we can save operations by computing derivation of Gaussian first.

$\because$ Differentiation is a convolution operation

$\therefore (h \otimes g) \otimes f$ where $g = \begin{bmatrix}1 & 0 & -1\end{bmatrix}$

##### Which $\delta$ should we use? (for Gaussian kernel)

Larger $\delta$  detects longer-scale edges, smaller $\delta$  detects finer edges.

Large $\delta \rightarrow$ Good detection (high SNR). Poor localization

Small $\delta \rightarrow$ Poor detection (low SNR). Good localization

**Smoothing to suppress noise. Increase contrast to enhance edges.**

### Canny Edge Detection

#### Edge Thining

Non-Maximum Suppression

For each pixel, check if it is a local max along the gradient direction. If so, keep as edge, if not, discard (set to 0).

#### Hysteresis Thresholding

Use a high threshold to start edge curves, and a low threshold to continue growing them.

1. Mark pixels that clear high threshold as boundary
2. If there are pixels that clear low threshold and are connected to a boundary, mark them as boundary too.

### Region

#### K-means Clustering

Randomly initialized the k cluster centers, and iterate between assigning membership and computing cluster centers

##### Pros

- Simple, fast to compute
- Converges to local minimum of within-cluster squared error

##### Cons/issues:

- Setting k
- Sensitive to initial centers
- Sensitive to outliers
- Detects spherical clusters
- Assumes means can be computed

#### Mean Shift Algorithm

Find modes or local maxima of density in the feature space

##### Kernel Density Estimation

A kernel is a function that satisfies:

1. $f_{R^d}\phi (x) = 1$
2. $\phi(x)≥0$

Examples:

1. Rectangular: $\phi(x) = 1$ for  $a ≤x≤b$
2. Gaussian: $\phi(x) = e^{-{x^2 \over 2\sigma^2}}$
3. Epanechnikov: $\phi(x) = {3 \over 4}(1-x^2)$ for $\mid x\mid ≤1$

##### Technical Details

$\hat {f_K} = {1 \over nh^d} \sum_{i=1}^nK({x-x_i\over h})$

$K(x) = c_kk(\| x \| ^2)$

$\nabla \hat f(x) = {2c_{k,d} \over nh^{d+2}} [\sum_{i=1}^ng(\| {x-x_i \over h} \|^2)][{\sum_{i=1}^n x_ig(\| {x-x_i \over h} \|^2) \over \sum_{i=1}^n g(\| {x-x_i \over h} \|^2)} -x]$

$K(x)$: the radially symmetric kernel to estimate density

$h$: bandwidth, defines the radius of kernel

$c_k$: a normalization constant

$g(x)$: $-k'(x)$ denotes the derivative of the selected kernel profile

We expect $\nabla \hat f(x)$ to be 0

${2c_{k,d} \over nh^{d+2}} [\sum_{i=1}^ng(\| {x-x_i \over h} \|^2)]$ unlikely to be 0, because it's proportional to density estimate at x

**The mean shift procedure from a give point $x_t$ is:**

1. Compute the mean shift vector m: $[{\sum_{i=1}^n x_ig(\| {x-x_i \over h} \|^2) \over \sum_{i=1}^n g(\| {x-x_i \over h} \|^2)} -x]$

2. Translate the density window: $x_i^{t+1} = x_i^t + m(x_i^t)$
3. Iterate steps 1 and 2 until convergence, i.e. $\nabla f(x_i) = 0$

##### Pros

- General algorithm for mode-finding (we apply it towards segmentation)
- Model-free; **no prior assumptions** on cluster shape or distribution of data (spherical, elliptical, etc.)
- One parameter (window size / bandwidth *h*, which has a physical meaning)
- Finds **variable number** of modes (vs. pre-specified k in k-means)
- **Robust to outliers**

##### Cons

- Output depends on *h* and selecting ***h* is non-trivial**
- **Computationally expensive**
- Scales poorly with feature space dimension

##### Computational Complexity Improvement

1. Assign all points **within radius $r$ of end point** to mode
2. Assign all points **within radius $r/c$ of search path** to mode