---
title: Digital Image Representations & Image Filtering
tags: [Computer Vision]
mathjax: true
---

### Computer Vision

#### Measurement

Computing **properties** of 3D world from **visual data**.

#### Semantics

Algorithms and representations to allow a **machine** to **recognize** objects, people, scenes, activities.

#### Organization & Search

Algorithms to **mine, search, and interact** with visual data

### Digital Image Acquisition

*Keyword: Energy* 

1. **Energy** transfer form sun, light bulb to scene to optical system
2. Lens focuses **energy** onto sensor
3. Digital sensor measures amount of **energy**, convert light into electrical charge

Energy -> Exposure (Brightness)

To express $n$ pixel intensities in an image: $sqrt(n)$ bits are required

#### Images in Python

image(y, x, b) = y pixels down, x pixels to right in the $b^{th}$ channel

#### RGB Colour Model - Additive

Electronic display & photography

Based on trichromatic human perception of colours 

Default colour space

#### Colour to Greyscale

$I = W_R * R + W_G * G + W_B * B$

$W_R + W_G + W_B = 1$  -> To keep values within [0, 255]

### Colour Spaces

#### RGB Colour Space

(0, 50 , 0)  (0, 100 , 0)  (0, 223 , 0) are the same colour but with **different illumination levels**

##### Normalize RGB

(R, G, B) <=> (r, g, I)

$r = R / (R + G + B)$

$g = G / (R + G + B)$

$b = 1 - r - g$

$I = (R + G + B) / 3$	is Illumination (amount of light) -> intensity carries more information

#### HSV Colour Space

Hue (色调): dominant wavelength in perceived light

Saturation (饱和度): amount of white light mixed with the pure colour

$H = ((G - B) / (V - min(R, G, B))) * 60°$	if $V = R$ and $G ≥ B$

​     $= (((B - R)/ (V - min(R, G, B))) + 2) * 60°$	if $V = G$

​     $= (((R - G)/ (V - min(R, G, B))) + 4) * 60°$	if $V = B$

​     $= (((R - B)/ (V - min(R, G, B))) + 5) * 60°$	if $V = R$ and $G ≤ B$

$S = (V - min(R, G, B)) / V$ for $S  \in [0, 1]$

$V = max(R, G, B)$ for $V \in [0, 255]$

#### YCbCr Colour Space

Y is the same as rgb to greyscale conversion

#### L\*a\*b\* Colour Space

L is the same as rgb to greyscale conversion

### Colour-Based Image Retrival

#### Training

Extract and store one\* color histogram per image

#### Testing

Extract the query image color histogram

Compute **intersection** between query histogram and each database histogram

Sort intersection values

Rank database items relative to query based on this sorted order

#### Anaysis

\+ No spatial informaiton: invariant to translation, rotation, scale

\-: Not very discriminative

#### Use

Colour-Based Skin Detection

Colour-Based Segmentation

### Point Processing

#### Adjusting Brightness

$x_{ij} = p_{ij} + b$

Clipping behaviour

#### Adjusting Contrast

$x_{ij} = a \times p_{ij}$

#### Image Normalization (Whitening)

$x_{ij} = (p_{ij} - \mu) / \delta$

Resulting image pixels are 0 mean, unit variance

#### Gamma Mapping

$x_{ij} = 255 \cdot (p_{ij} / 255)^\gamma$	$\gamma > 0$

When $\gamma$ <1: increasing mid-levels increases the dynamics in the dark areas

when $\gamma$ > 1: depressing mid-levels increases the dynamics in the bright areas

#### Logarithmic & Exponential Mappings

### Image Histogram

- Information is global — Pixel locations don't matter

- Histogram Stretching

- Histogram Equalization: Forces the histogram of intensities to be the same

  Equalization can deal with the situation that low contrast image with pure white/black pixels inside it, i.e. cannot be stretched.

- Histogram is good for thresholding to detect object and background, but only for those in which objects and background are separated in image.

### Filtering

Motivation: noise reduction

#### Common types of noise

1. Salt and pepper noise

   random occurrences of black and white pixels

2. Impulse noise

   random occurrences of white pixels

3. Gaussian noise

   variations in intensity drawn from a Gaussian distribution

   $p_{ij} = \hat p_{ij} + \eta$ 

   observation = ideal image + noise

#### Correlation filtering

Averaging window size: 2k+1 x 2k+1

##### Averaging Filtering (Box Filtering)

Uniform weight: $x_{ij} = {1 \over 2k+1 }^2 \sum_{u=-k}^k\sum_{v=-k}^kp_{i+u, j+v}$

Can be used to smooth picture

The larger the filter is, the more blurred the image is.

##### Linear Filtering

Different weights (**Cross-correlation**): $\sum_{u=-k}^k\sum_{v=-k}^kf_{uv}\cdot p_{i+u, j+v}$

​	$f_{uv}$ is the weight depends on neighbour's relative position wrt $p_{ij}$

​	$X = F \otimes P$, $F$ is filter kernel

##### Gaussian Filtering (Low-Pass Filter)

$f_{uv} = {1\over 2\pi\delta^2}e^{-{u^2+v^2\over\delta^2}}$ 

Removes high-frequency components from the image

$\delta^2$ variance determines extent of smoothing

##### Median Filter

Sort the neighbours of a pixel with itself, pick the median value as its new value

- No new pixel values introduced
- Removes spikes: good for impulse, salt & pepper noise

#### Possibilities for Boundaries

1. zero-padding
2. wrap around
3. copy edge
4. reflect across edge

#### What is filtering good for

1. Enhance an image (denoising, sharping, etc)
2. Extract information (texture, edges, etc)

3. Find patterns (template matching)

### Convolution vs. Cross-correlation

Flip the kernel in both dimensions, then apply cross-correlation

**Convolution**

$x_{ij} = \sum_{u=-k}^k\sum_{v=-k}^kf_{uv}\cdot p_{i-u, j-v}$

$X = F * P$

**Cross-correlation**

$x_{ij} = \sum_{u=-k}^k\sum_{v=-k}^kf_{uv}\cdot p_{i+u, j+v}$

For Gaussian or box filter, they are the same

