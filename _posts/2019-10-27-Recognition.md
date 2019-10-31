---
title: Recognition
tags: [Computer Vision]
mathjax: true
---

### Recognition

#### Feature Matching

An object as a collection of bag of features

- deals well with occlusion 
- scale invariant 
- rotation invariant 
- position of parts is not important
- Works well for object *instances*
- Not great for generic object *categories* 

##### Pros

- Simple
- Efficient algorithms
- Robust to deformations

##### Cons

- No spatial reasoning

##### Bag Of Words

Works pretty well for image-level classification, less okay for object detection because spatial layout gets lost and objects are with shape and boundaries

Present a data item (document, texture, image) as a histogram over features

##### **T**erm **F**requency **I**nverse **D**ocument **F**requency (TFIDF)

$TF = n(w_i, d) =  { \#~of~w_i~appearance \over \#~of~all~word~d}$ 

$IDF = log \{ {D \over \sum_d' 1[w_i \in d']}\} = log \{ {\#~of~all~documents \over \#~of~all~documents ~has~this~word~w_i } \}$

$TFIDF = n(w_i, d)\alpha_i = n(w_i, d) log \{ {D \over \sum_d' 1[w_i \in d']}\} = TF * IDF$

##### Texture recognition

Characterized by repetition of basic elements or *textons*

Assumes that each texture is sufficiently represented purely by the occurrence distribution of the textons.

#### Standard BOW Pipeline

**Dictionary Learning**: Learn Visual Words using clustering

##### Which Features to Extract

BOW aggregates **local information**

**SIFT** (the descriptor) is preferred due to its discriminativeness

- To **find specific textured objects**, sparse sampling from interest points often more reliable. 

- Multiple complementary interest operators offer more image coverage. 
- For object categorization, dense sampling offers better coverage. 

Clustering is a common method for learning a visual vocabulary or codebook

If the training set is **sufficiently representative**, the codebook will be universal

**Encode**: Build Bags-of-Words (BOW) vectors for each image

- Quantization: associate each image feature to a visual word (nearest cluster center)
- Histogram: count the number of visual word occurrences

**Classify**: Train and test data using BOWs

**Pros**

- flexible to geometry / deformations / viewpoint

+ compact summary of image content
+ provides fixed dimensional vector representation for sets
+ very good results in practice

**Cons**

- background and foreground mixed when bag covers whole image 
- optimal vocabulary formation remains unclear 
- basic model ignores geometry – must verify afterwards, or encode it somehow within via features 

#### kNN

Important to **normalize**: dimensions have different scales

Hyper-parameters are highly problem-dependent, need to choose via validation data.

**Pros** 

- simple yet effective 

**Cons** 

- search is expensive (can be sped-up) - O(mn)
- storage requirements - O(m)
- difficulties with high-dimensional data - comparison of distance measure becomes more difficult with higher-dimensional data

#### Spatial Reasoning

1. Extract features

2. Match features
3. Spatial verification

**Pros** 

- Retains spatial constraints
- Robust to deformations 

**Cons** 

- Computationally expensive
- Generalization to **large** inter-class variation (e.g., modeling chairs) 

#### Window Classification

Template Matching

1. Get image window
2. Extract features
3. Compare with template

**Pros** 

- Retains spatial constraints
- Can be highly discriminative 

**Cons** 

- Many many possible windows to evaluate
- Requires large amounts of data
- Sometimes (very) slow to train 

#### Window-Based Object Detection Framework

1. Build/train object model
   - Choose a representation
   - Learn or fit parameters of model / classifier

2. Generate candidate windows in new image 

   -  test all locations and scales

   Scale the image instead of the template

3. Score the candidate

4. Resolve detections (NMS)

   - Rescore candidates based on entire set

   Threshold & apply non-maximum suppression

##### Why scale the image and not the template?

Template is complicated and **scale-specific**; unlikely to survive a resizing
operation, so we scale input image instead.

When it is simple, e.g. Viola-Jones, it is okay (and actually more efficient) to rescale the detector / template / features.

##### Evaluation

- Area of Overlap

  $AO(B_{gt}, B_p) = {B_{gt} \cap B_p \over B_{gt} \cup B_p}$

- Average Percision

  Averages precision over the entire range of recall

  A good score requires both high recall and high precision

##### Deal with different viewpoint

Often train different models for a few different viewpoints

#### Support Vector Machines (SVM)

**Maximum Margin solution:** most stable to perturbations of data

**Margin:** the gap between parallel hyperplanes $2\over \parallel w \parallel$ is maximized

Trade-off between the **margin** and the **mistakes**

##### Multi-class SVMs

###### One vs. all

- Training: learn an SVM for each class vs. the rest 

- Testing: apply each SVM to test example and assign to it the class of the SVM that returns the highest decision value 

###### One vs. one

- Training: learn an SVM for each pair of classes
- Testing: each learned SVM “votes” for a class to assign to the test example

##### Convert the Classification Score to Confidence Score

$y = {1 \over 1 + e^{Ax+B}}$

$A$ and $B$ are fitted based on classifier outputs (i.e. from validation data) s.t. the same classifications result

**Pros**

- Widely available and built into virtually any machine learning package Kernel-based framework is very powerful, flexible 
- Compact model 
- Work very well in practice, even with very small training sample sizes 

**Cons**

- No “direct” multi-class SVM, must combine two-class SVMs 
- Can be tricky to select best kernel function for a problem 
- Computation, memory
  - During training time, must compute matrix of kernel values for every pair of examples
  - Learning can take a very long time for large-scale problems 

#### Dalal-Triggs Pedestrian Detector

applying SVMs to window-based object detection using HoG (histogram of oriented graidents).

**Object model** = sum of scores of features **at fixed positions**!

1. Extract fixed-sized (64x128 pixel) window at each position and scale 

   Normalize gamma & color

2. Compute HOG (histogram of gradient) features within each window 

   Orientations: 9 bins (for unsigned angles 0 - 180)

   - Votes weighted by magnitude
   - Bilinear interpolation between cells
   - Histograms over k x k pixel cells (similar to SIFT)

   Contrast normalized over overlapping spatial blocks

   - Normalize wrt surrounding cells
   - Concatenate all cell responses from block into vector 
   - Normalize vector
   - Extract responses from cell of interest
   - Do this 4x for each overlapping block

   $\#features= \# cells \times  \# orientations \times \# normalizations~by
   ~neighboring~cells$

3. Score the window with a linear SVM classifier 

4. Perform non-maxima suppression to remove overlapping detections with lower scores 

##### Pedestrian detection with HOG 

- Learn a pedestrian template using a SVM
- At test time, compare feature map with template over sliding windows.
- Find local maxima of response
- Multi-scale template: repeat over multiple levels of a **HOG pyramid** 

**Pros**

- Sliding window detection and global appearance descriptors follow a simple implementation protocol 
- Good feature choices critical 
- Past successes for certain classes 

**Cons**

- High computational complexity
  - $\#locations \times \#orientations \times \#scales$ evaluations!
  - If training binary detectors independently, cost increases linearly with number of classes 
- FP rate can be high

**Limitations**

- Some objects are not box shaped
- Non-rigid, deformable objects not captured well with representations assuming a fixed 2d structure; or must assume fixed viewpoint 
- Objects with less-regular textures not captured well with holistic appearance-based descriptions 
- If considering windows in isolation, context is lost
- In practice, often entails large, cropped training set (expensive) 
- Requiring good match to a global appearance description can lead to **sensitivity to partial occlusions** 

#### Tricks of the trade

- Template size 
  - Typical choice is size of smallest detectable object
- “Jittering” to create synthetic positive examples
  - Create slightly rotated, translated, scaled, mirrored versions as extra positive examples
- Bootstrapping to get hard negative examples
  - Randomly sample negative examples 
  - Train detector 
  - Sample negative examples that score > -1 
  - Repeat until all high-scoring negative examples fit in memory  

#### Viola-Jones Face Detector

Slow to train but detection is very fast

1. *Integral images* for fast feature evaluation

   $I_\sum (x, y) = \sum_{x'≤x, y'≤y} i(x', y')$

   So that only 3 additions are required for any size of rectangle

2. *Boosting* for feature selection

   Features that are fast to compute - “Haar-like features” 

   - Differences of sums of intensity $Value = \sum (pixels~in~white~area) -\sum(pixels~in~black~area) $
   - Computed at different positions and scales within sliding window 

   In each boosting round:

   - Find the weak classifier that achievesthe lowest *weighted* training error.

   - Raise the weights of training examples misclassified by current
     weak classifier.
   - Compute final classifier as linear combination of all weak classifier. 
   - Weight of each classifier is directly proportional to its accuracy.

   $h(x) = sign(\sum^M_{j=1}\alpha_jh_j(x))$

   $h$: final strong learner

   $x$: window

   $\alpha_j$: learner weight

   $h_j$: weak learner

   Choose weak learner that minimizes error on the weighted training set, then reweight

3. *Attentional cascade* for fast non-face window rejection

   Reject many nagative examples but detect almost all positive examples

   - Set target detection and false positive rates for each stage 
   - Keep adding features to current stage until target rates have been met 
     - Need to lower boosting threshold to maximize detection (as opposed to minimizing total classification error) 
     - Test on a *validation set* 
   - If the overall false positive rate is not low enough, add another stage 
   - Use false positives from current stage as the negative training examples for the next stage 
   
   Chain classifiers that are progressively more complex and have lower false positive rates

##### Boosting vs. SVM

**Advantages of boosting** 

- Integrates classifier training with feature selection 
- Complexity of training is linear instead of quadratic in the number of training examples 
- Flexibility in the choice of weak learners, boosting scheme 
- Testing is fast 

**Disadvantages**

- Needs many training examples
- Training is slow
- Often doesn’t work as well as SVM (especially for many-class problems) 

For 1 Mpix, to avoid having a false positive in every image, our false positive rate has to be less than $10^{-6}$

##### Summary

- Sliding window for search 
- Features based on differences of intensity (gradient, wavelet, etc.) 
  - Excellent results require careful feature design 
- Boosting for feature selection 
- Integral images, cascade for speed 
- Bootstrapping to deal with many negative examples 

