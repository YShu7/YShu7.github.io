---
title: Frequent Pattern Matching
tags: [Machine Learning]
mathjax: true
---

### Frequent Pattern Matching

- Compare the corresponding two writing patterns for a match

**writing pattern**: the combination of features that occur frequently in his/her emails.

To define writing pattern of an author, we use **frequent pattern**

**The support of F**: the percentage of emails that contains F

$support\{F\} = {number~of~emails~containg~F \over total~number~of ~emails}$

A **frequent pattern** F in a set of emails is said to occur when $support\{F\}≥t$

$SSCORE = {number~of~common~frequent~patterns \over total~number~of~possible~frequent~patterns}$

**Step 1**: Assuming two unknown identities (A and B), and n emails each from A and B, extract the feature values as discussed before.
**Step 2**: Encode the feature values into feature items. Compute the frequent pattern for each identity according to the minimum support threshold t and pattern order k. Compute the common frequent pattern number and SSCORE.
**Step 3**: Compute the correct identification rate using leave one out cross validation based machine learning method (eg. decision tree). After running 2n identifications, the average identification rate is $DSCORE = {number~of ~correct ~identification \over 2n}$ is computed.
**Step 4**: The final score S = αSSCORE+(1-DSCORE) where α is a parameter chosen to trade-off importance given to similarity vs. difference detection.
Step 5: Set a threshold T, and compare S with T. If S > T , the two unknown identities are the same person. If S ≤ T , the two identities are different person.