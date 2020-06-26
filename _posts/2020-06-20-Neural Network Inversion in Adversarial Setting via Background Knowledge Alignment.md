# Neural Network Inversion in Adversarial Setting via Background Knowledge Alignment

## Model Inversion

A technique that aims to obtain information about the training data from the model's predictions.

### Optimization-based Approach

Inverts a model by making use of gradient-based optimization in the data space.

Doesn't really capture the semantics of the data space.

Require while-box accesses to the classifier to compute the gradients.

#### Model Inversion Attack (MIA)

It casts the inversion task as an optimization problem to find the 'optimal' data for a given class.

Works for simple network but is shown to be ineffective against complex neural networks such as CNN.

### Training-based Approach

Inverts a model by learning a second model that acts as the inverse of the original one.

Aims at reconstructing images from their computer vision features including activations in each layer of a NN. Threfore, they train the model using full prediction vectors on the same training data of the classifier.

Use the same training data to train the "inversion" model.

## Adversaruak Scenario - Inversion Attack

An adversary is given **the classifier and the prediction result**, and aims to make sense of **the input data and/or the semantics of the classification**.

### Inversion Settings

#### Data Reconstruction

Reconstruct unknown data given the classfier's prediction vector on it.

e.g. the person's identity in a facial image -> reconstruct the facial image of a person

#### Training Class Inference

Recover a semantically meaningful data for each training class of a trained classifier.

e.g. a recognizable facial image of an arbitrary person in the training data.

### Adversarial Settings

- The attacker does not have the classifier's training data.

- The classifier is black-box.

- The adversary might obtain only partial prediction results on victim data.

  > The partial predictions largely limit the reconstruction ability of the inversion model
  >
  > Although the reconstruction is accurate on the full prediction vector, a slight deviation, such as truncation in the prediction vector, will lead to a dramatically different result.