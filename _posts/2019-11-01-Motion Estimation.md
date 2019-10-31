### Motion Estimation

|                                  | Feature Matching                                             | Object Detection                                             |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| scoring                          | **Feature matching** across pairs of images and not feature detection (e.g. cornerness score). <br/>A **lower score is a better match**, since we use distance measure for comparison. | Higher -> more likely to be an object                        |
| threshold                        | Keep matches if **below** some threshold                     | Keep detections if above some threshold                      |
| Precision<br/>$TP \over  TP+FP$  | How accurate are the feature pairs declared as matches?<br/>Lower threshold -> lower FP (also lower TP, but doesn't matter) -> higher precision | How accurate are the detections?<br/>Higher threshold -> lower FP -> high<br/>precision |
| Recall<br/>$TP \over TP+FN$      | Was the algorithm able to find all the actual pairs of features?<br/>Higher threshold -> more TP (again balanced in numerator and denominator) butalso lower FN -> higher recall | Could we find all the objects?<br/>Lower threshold -> less FN -> higher recall |
| Specificity<br/>$TN \over TN+FP$ | Can the algorithm correctly disregard the features which are not part of any pair?<br/>Lower threshold -> lower FP, no impact on the TNs -> higher specificity |                                                              |

If threshold too high:

high precision: few false positives

low recall: many false negatives

#### Motion Estimation

##### Key Assumptions

- Color Constancy 

  Brightness constancy for intensity images

  **Implication**: allows for pixel to pixel comparison (not image features)

  $I(x(t), y(t), t) = C$

- Small Motion

  Pixels only move a little bit

  **Implication**: linearization of the brightness constancy constraint

  $I(x + u\delta t, y+v\delta t, t + \delta t) = I(x, y, t)$

##### Approach

Look for nearby pixels with the same color

$I(x + u\delta t, y+v\delta t, t + \delta t) = I(x, y, t)$

$I(x, y, t) + {\partial I \over \partial x}\delta x + {\partial I \over \partial y}\delta y + {\partial I \over \partial t}\delta t = I(x, y, t)$

${\partial I \over \partial x}\delta x + {\partial I \over \partial y}\delta y + {\partial I \over \partial t}\delta t = 0$

$I_xu + I_yv + I_t = 0$

| Horn-Schunck Optical Flow                                    | Lucas-Kanade Optical Flow                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| brightness constancy, small motion                           | method of differences                                        |
| **smooth flow** (flow can vary from pixel to pixel)          | **constant flow** (flow is constant for all pixels)          |
| global method<br/>(dense)                                    | local method<br/>(sparse)                                    |
| Direct, dense methods<br/>- Directly recover image motion at each pixel from spatio-temporal image brightness variations<br/>- Dense motion fields, but sensitive to appearance variations<br/>- Suitable for vedio and when image motion is small | Feature-based methods<br/>- Extract visual features (corners, textured area) and track them over multiple frames<br/>- Sparse motion fields, but more robust tracking<br/>- Suitable when image motion is large (10s of pixels) |

#### Lucas-Kanade Optical Flow

##### Assumption

$I_xu + I_yv + I_t = 0$

Assume that the surrounding patch has **constant flow**

$\begin{bmatrix} I_x(p_1) & I_y(p_1) \\ I_x(p_2) & I_y(p_2) \\ \vdots &\vdots \\ I_x(p_{25}) & I_y(p_{25})\end{bmatrix} \begin{bmatrix}u \\ v\end{bmatrix} = -\begin{bmatrix} I_t(p_1) \\ I_t(p_2) \\ \vdots \\ I_t(p_25) \end{bmatrix}$

$Ax = b$

Least Squares Approximation: $A^TA\hat x = A^Tb$

$x = (A^TA)^{-1}A^Tb$

- $A^TA$ should be invertible
- $A^TA$ shouldn't be too small ($\lambda_1$ and $\lambda_2$ shouldn't be too small)
- $A^TA$ should be well conditioned ($\lambda_1 / \lambda_2$ shouldn't be too large)

> $A^TA$ was introduced in Harris Corner Detector
>
> - Corners are when λ1, λ2 are big; this is also when Lucas-Kanade optical flow works best 
> - Corners are regions with two different directions of gradient (at least)
> - Corners are good places to compute flow! 

##### Aperture Problem

Small visible image patch of line cannot tell the direction of movement

Want patches with **different gradients** to the avoid aperture problem 

##### Aliasing

Temporal aliasing causes ambiguities since images can have many pixels with the same intensity and lead to wrong ‘correspondences’

##### Coarse-to-Fine Optical Flow Estimation

run iterative L-K -> wrap & upsample -> run iterative L-K -> ...

#### Horn-Schunck Optical Flow

For every pixel,

**Enforce brightness constancy:** $min_{u,v}[I_xu_{ij} + I_yv_{ij} + I_t]^2$

**Enforce smooth flow field:** $min_u(u_{i,j}-u_{i+1,j})^2$

$min_{u,v}\sum_{i,j}\{E_s(i,j)+\lambda E_d(i,j)\}$

$E_s(i,j) $: smoothness

$E_d(i,j)$: brightness constancy

$\lambda$: weight

Compute partial derivative, derive update equations

#### Applications for Optical Flow

- Segmentation of objects in space or time 
- Estimating 3D structure 
- Learning dynamical models – how things move 
- Recognizing events and activities 
- Improving video quality

##### Errors in assumptions 

- A point does not move like (all) its neighbors, e.g. at object boundaries 
- Brightness constancy does not (always) hold 
- The motion is large (larger than a pixel) 
  - Not-linear: Iterative refinement
  - Local minima: coarse-to-fine estimation 