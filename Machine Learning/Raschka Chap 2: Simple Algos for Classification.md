# Raschka Chap 2: Simple Algos for Classification

**Artificial Neurons**

At the very beginning, in order to design an artificial intelligence, researchers looked into how real neurons work. We can think of a neuron as a logic gate: multiple signals arrive, and if there are enough of them, an output signal is generated. The output is therefore binary. 

This artificial neuron can be used in the context of outputting 0 or 1. We have a decision function $\sigma(z)$ that takes in $z = w_1x_1 + ... + w_mx_m$ where $w_i$ are the weights and $x_i$ are the features, and the output rule is 

$$\sigma(z) = \begin{cases}
1, & x \geq \theta \\
0, & \text{else}
\end{cases}$$
If we define the bias $b = -\theta$ and rewrite everything as vectors, we can let the input be $z=w^Tx+b$ and the rule would be simply to output 1 if $z \geq 0$. 

**Perceptron learning rule**

Here is the basic learning algorithm:

- Initialize the weights and bias to 0 or small random numbers
- For each training example $x^{(i)}$ :
	- Compute the output value $\hat{y}^{(i)}$
	- Update the weights and bias
- Repeat

Clearly the hard part is figuring out how to update the weights and bias in a sensible manner. Here is the formula to update them.

$$w_j = w_j + \Delta w_j$$ 
and $$b = b + \Delta b$$
where the deltas are computed as 

$$\Delta w_j = \eta (y^{(i)} -\hat{y}^{(i)})x_j^{(i)}$$

and $$\Delta b = \eta(y^{(i)} -\hat{y}^{(i)})$$
The constant $\eta$ is called the learning rate and is typically between 0 and 1. Also, $y^{(i)}$ is the true class label, while $\hat{y}^{(i)}$ is the predicted class label.

The main idea behind the perceptron rule is to push the weights and bias towards the opposite direction whenever the predicted value is wrong. Scaling the deltas in terms of the feature $x_j^{(i)}$ makes sense because if the feature is large, it should contribute more, so you need to shift the weight by more so that it classifies it correctly next time.

Note that the perceptron model will only converge if the two classes can be perfectly separated by a linear boundary. If we are not guaranteed convergence, we can set a maximum number of passes over the training dataset, or a threshold for number of tolerated misclassifications.

Here's a diagram summarizing how the perceptron works:

![[Pasted image 20260209224230.png]]

**Implementation of the Perceptron**

The implementation can be found at this link: https://github.com/mathcslearner/ML_Implementation

A few notes on the implementation:

- The initial weight vector is created using small random numbers from a normal distribution with std dev of 0.01 . We use a seed so that results can be reproduced if desired.
- The arithmetic operations in NumPy are vectorized, which means any operation is automatically applied to all elements in the array. This is why the dot product actually runs faster than multiplying using say list comprehension.

**Training a perceptron**

In this section, we will train the perceptron model we just implemented on the Iris dataset to observe its performance. 

The training can be found at this link: https://github.com/mathcslearner/ML_Implementation

Here are the basic steps of the training:

- Get the dataset
- Visualize the scatterplot of petal length vs sepal length, as well as the classes
- Initialize a perceptron model and train it on the data
- Plot the decision region obtained from the trained perceptron

**Adaptive linear neurons**

This is another kind of single-layer neural network: the ADAptive LInear NEuron (Adaline). It is designed to be an improvement on the perceptron.

The Adaline algorithm is focused on minimizing a continuous loss function, rather than number of misclassifications. The weights are updated based on the function $\sigma(z) = z$. Note that we still keep $z = w^Tx + b$. To make the final prediction, we still use the step function. Here is a diagram illustrating how the adaline works:

![[Pasted image 20260210164019.png]]

During training, this model compares the true class labels with the output of the activation function, rather than the predicted class labels.

The optimization is done using a loss function which represents the mean squared error (MSE) between calculated and true class label. The goal during training is to minimize this loss function:
$$L(w,b) = \frac{1}{2n}\sum_{i=1}^n(y^{(i)}-\sigma(z^{(i)}))^2$$
This loss function is differentiable and convex, so we can use gradient descent to find suitable weights to minimize it. The idea behind gradient descent is to start at a certain input. For each iteration, we take a step in the opposite direction of the gradient, so that we approach a local minimum. Here's the formula, we have $w = w + \Delta w$ and $b = b + \Delta b$ where
$$\Delta w = -\eta \nabla_w L(w,b), \space \Delta b = - \eta \nabla_b L(w,b)$$
and we can compute the gradient by computing the partial derivative as
$$\frac{\partial L}{\partial w_j} = - \frac{2}{n}\sum_i (y^{(i)}-\sigma(z^{(i)}) x_j^{(i)}$$
and 
$$\frac{\partial L}{\partial b} = - \frac{2}{n}\sum_i (y^{(i)}-\sigma(z^{(i)})$$
Note that in the adaline algorithm, we update the weight based on all examples in the dataset at once, rather than processing one example at a time. This is known as full batch gradient descent.

The implementation can be found here:  https://github.com/mathcslearner/ML_Implementation

Notes on the implementation:

- Obviously, here the activation function does nothing as it is simply the identity. However, for general single-layer NN, for example logistic regression, it might be different, so it is good to include it in our code.
- Again, always use dot products rather than for loops since numpy is optimized for this
- We can use the loss values in self.losses_ to check whether the algorithm converges or not

In practice, choosing a good learning rate is very important so that the algorithm converges as fast as possible. If it is too large it will keep overshooting, if it is too small it will take too long. The learning rate $\eta$ and the number of epochs are both examples of hyperparameters.

We can improve gradient descent by doing a feature scaling method called standardization. It makes it converge faster. Standardization involves shifting the features so that the mean is zero, and the std dev is 1. For the jth feature, the standardized version is
$$x_j' = \frac{x_j - \mu_j}{\sigma_j}$$
We apply this standardized version to all features. The basic idea is that if the features are on different scales, they might need different learning rates to update them. By putting them on the same scale, we can use a single rate that works for all. After standardizing, eta=0.5 should be good to use.

In Numpy, we standardize using .mean() and .std().

**Stochastic gradient descent**

The problem with full batch gradient descent is that if we have a very large dataset with millions of points, it is computationally costly to be evaluating the model on the whole dataset at every iteration. 

The solution is to use stochastic gradient descent (SGD). Instead of updating the weights based on the overall accumulated error, we update it incrementally based on the error for each training example, as such:
$$\Delta w_j= \eta (y^{(i)}-\sigma(z^{(i)})) x_j^{(i)}$$
and
$$\Delta b = \eta (y^{(i)}-\sigma(z^{(i)}))$$
Since we are updating the weights more frequently, the algorithm also converges faster. 

To obtain good results, we should give the training data in a random order. We can also shuffle it before each epoch. An advantage of SGD is that it can be used for online learning, where data comes in one at a time.

Another option is mini-batch gradient descent, which is a compromise between the two. You give smaller batches of training data at a time to the model.

Here are a few notes on the implementation (which is mostly similar to the basic Adaline model):

- There are two fitting functions, which are fit() and partial_fit()
- The main fit() function is the traditional SGD where in each epoch, we pass in one training data point at a time to train it. This function trains the model from scratch.
- The partial_fit() function is used for incremental learning, where we can call it repeatedly by providing data points in small batches.
