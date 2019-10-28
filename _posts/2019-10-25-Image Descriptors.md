---
title: Image Descriptors
tags: [Computer Vision]
mathjax: true
---

### Image Descriptors

Represent surrounding region mathematically

- Color histogram
- Mean dy/dx values
- Texton histograms

Feature Descriptors should be

- Invariant: shouldn’t change even if image is transformed
- Discriminative: should be highly **unique** for each point

#### Raw Image Patch: Intensity

Not invariant to geometry and appearance change

#### Image Gradients

Sensitive to deformations

#### Colour Histogram

Invariant to changes in scale and rotation

Not sensitive to the spatial layout

#### Spatial Histogram

Retains rough spatial layout

Some invariance to deformations

Not invariant to rotation

#### Multi-Scale Oriented Patches (MOPS)

1. Take 40x40 window around detected interest point (e.g. Harris corner) 

2. Subsample every 5th pixel $\rightarrow$ absorbs localization errors 

3. Rotate to horizontal $\rightarrow$ robustness to photometric variations

4. Normalize intensity values within the window by **subtracting the mean & dividing by the standard deviation**. 

#### GIST Descriptor

1. Divide image patch into 4x4 cells 
2. Apply filter bank (Gabor filters) of directional edge detectors.
3. Compute filter response averages for each cell. 
4. Size of descriptoris 4x4xN, where N is size of the filter bank. 

GIST encoded rough spatial distribution of the image gradients!

##### Gabor filters

If scale is small compared to the frequency, the Gabor filters can be used
as **approximate derivative operators**

odd Gabor filter looks a lot like Gaussian Derivative

even Gabor filter looks a lot like Laplacian (2nd derivative)

#### Scale Invariant Feature Transform (SIFT)

1. Take 16x16 window around detected interest point

   window: normalized wrt scale, orientation, and intensity

2. Divide the 16x16 window into a 2x2 case

   to add some sensitivity to spatial layout

3. Compute edge orientation (angle of the gradient - 90°) for each pixel 

4. Throw out weak edges (threshold gradient magnitude) 

5. Create histogram of surviving edge orientations for each cell

6. 16 cells * 8 orientations = 128 dimensional descriptor

   normalized to 1

##### Advantages:

- Invariant to scale & rotation 

- Can handle changes in viewpoint
  - Up to about 60 degree out of plane rotation 
- Can handle significant changes in illumination
  - Sometimes even day vs. night (below) 
- Fast and efficient
  - SURF is an even faster follow-up derived from SIFT

### Feature Matching

#### Feature distance

ratio distance = $\parallel f_1-f_2 \parallel / \parallel f1-f_2' \parallel$

matching SIFT descriptros: threshold ratio of nearest to 2nd nearest descriptor

##### Evaluating Feature Matches

$recall = {\#TP \over \# TP + \# FP} $

$specificity = {\#FP \over \# TN + \# FN} $

$AUC$ Area Under the Receiver Operator Characteristic (ROC) Curve, 1 is the best

### Image Projections

#### Image transformations

**Filtering**: $G(x) = h\{F(x)\}$ Changes range of image function (pixel values)

**Wrapp**: $G(x) = F(h\{x\})$ Changes domain of image function (pixel locations)

#### Homography

1. Convert to homogeneous coordinates

2. Multiply by the homography matrix

   Special transformation with **8 degrees of freedom** relating the two projections. 

3. Convert back to heterogeneous coordinates

#### Direct Linear Transform (DLT)

$P' = H \cdot P$ 

One way to find $H$ is via DLT.

$x' = \alpha (h_1x + h_2y + h_3)$

$y' = \alpha (h_4x + h_5y + h_6)$

$1 = \alpha (h_7x + h_8y + h_9)$

So that

$h_7xx' + h_8yx' + h_9x' - h_1x - h_2y - h_3 = 0$

$h_7xy' + h_8yy' + h_9y' - h_4x - h_5y - h_6 = 0$

Re-write in matrix form: $A_ih = 0$

##### Solving for H using DLT

Given $\{x_i, x_i'\}$ solve for H such that $x' = Hx$

1. For each correspondence, create 2x9 matrix $A_i$
2. Concatenate into single 2n x 9 matrix $A$
3. Compute singular value decomposition (SVD) of $A=U\sum V^T$
4. Store singular vector of the smallest singular value $h=v_i$
5. Reshape to get H

##### Warping

Pixels may end up between two points

To dtermine the intensity of each point, we distribute color among neighboring pixels

If a pixel (x’,y’) receives intensity from more than one pixels (x,y), we average their intensity contributions.

##### Drawbacks

- linear least squares estimation, works only when the actual transform between two functions is linear
- doesn't deal with outliers $\rightarrow$ RANSAC

#### Random Sample Consensus (RANSAC)

1. Sample (randomly) the number of points required to fit the model 
2. Solve for model parameters using samples 
3. Score by the fraction of inliers within a preset threshold of the model 

#### Estimating homography with RANSAC

RANSAC loop

1. Get (min4) point correspondences (randomly)
2. Compute H using DLT

3. Count inliers

4. Keep H if largest number of inliers

Recompute H using all inliers 

