# ML Classifiers

For each particular problem, some classification algorithm might work better than others based on their assumptions. The performance of the classifier depends heavily on what the data looks like. Here are the five main steps:

- Selecting features, collecting labeled training examples
- Choosing a performance metric
- Choosing a learning algorithm and training the model
- Evaluating the performance
- Tuning the model

Scikit-learn is a Python library which provides an API for highly optimized implementations of several models.

Encoding class labels in numbers is good for classifier models. To evaluate how well the model performs on unseen data, we commonly split the dataset into training and test datasets. We can use train_test_split function for this. This function shuffles the dataset beforehand. Use stratify to make the train, test subsets have same proportion of class labels as input dataset.

To do scaling on the dataset (for example standardization, use StandardScaler). Use sc.fit() to compute the mean and std dev.

In scikit-learn, we can use accuracy_score to compute the classification accuracy.

## Logistic Regression

One of the main problems with the perceptron rule is that it never converges if the classes are not perfectly linearly separable.

Another more powerful algorithm for binary classification problems is logistic regression.

Consider $p$ the probability of an element being in a specific class. We can define $$\text{logit}(p) = \log \frac{p}{1-p}$$
In the logistic model we assume that there is a linear relationship between the weighted input and the log-odds, that is:
$$\text{logit}(p) = w^Tx+b$$
The idea is that given an input $x$, we want to adjust $w, b$ such that the probability $p$ is mostly correct in predicting the class. To do so we need to invert the logit function, which gives the sigmoid function
$$\sigma(z) = \frac{1}{1+e^{-z}}$$
where $z = w^Tx+b$ is the weighted input. The sigmoid function always outputs between 0 and 1.

The architecture of the logistic regression is identical to the adaline, except the activation function is the sigmoid instead of the identity. Here's a picture:

![[Pasted image 20260214205943.png]]

The threshold function is simply 1 if $\sigma(z) \geq 0.5$, 0 otherwise.

Similar to Adaline, we will be learning the model weights using a loss function. We can define the likelihood L of our model, which we want to maximize. To make it easier, we can convert it to a log likelihood and add a negative sign so that we can just do gradient descent as before. The end result is that we want to minimize

![[Pasted image 20260214210731.png]]

For a single training example, we see that this loss function is equal to $-\log(\sigma(z))$ if y=1 and $-\log(1-\sigma(z))$ is y=0.

To implement logistic regression, we can just use the same code for adaline, except we have to change the loss function and the activation function.

Our implementation only handles two classes. Using scikit-learn's API, we can actually use it on several classes. When using the API, we can choose what optimization algorithm to use. We used stochastic gradient descent in our implementation, but it's better to use more advances ones, such as BFGS.

## Overfitting

Overfitting is a common problem in ML, which happens when a model performs well on training data but does not generalize well to unseen data. This typically happens when the model is high variance and has too many parameters, leading to it being too complex for the data. If a model is underfitting, it has high bias, so not enough parameters, which also leads to low performance on unseen data.

The bias-variance tradeoff is the idea that we want to minimize both of them for the best model. Here's a good diagram illustrating what's going on:

![[Pasted image 20260214213010.png]]

To find a good bias-variance tradeoff, we can tune the complexity of the model using regularization. The idea is to introduce additional information to penalize extreme parameter weights. The most common one is L2 regularization:
$$\frac{\lambda}{2n}||w||^2$$
Here $\lambda$ is the regularization parameter. Now we can modify the loss function by adding on this term, which will shrink the weights during model training.

![[Pasted image 20260214215030.png]]

In the scikit-learn API, the parameter C which we passed to the logistic regression is inversely proportional to lambda. That is, decreasing C means increasing the regularization strength.

Be careful to not regularize too hard, otherwise it might lead to underfitting as all weight coefficients approach 0.

## Support Vector Machines

The SVM can be viewed as an extension of the perceptron. In perceptron algorithm, we want to minimize misclassification. In SVMs, the objective is to maximize the distance between the decision boundary and the training examples. The closest training examples are known as support vectors. Here's a diagram:

![[Pasted image 20260216133059.png]]

The idea is that having a larger margin leads to lower generalization error. In the API call to the SVM, decreasing the value of C increases the bias(underfitting), whereas increasing it increases the variance(overfitting) of the model.

In practice, linear logistic regression and linear SVMs often yield similar results. The logistic regression is simpler and easier to explain.

The main reason why SVMs are popular, then, is because they can be kernelized to solve nonlinear classification problems.

We can illustrate the use of kernel methods on a XOR dataset with random noise. Basically, plot a bunch of (x,y) points, and a point is blue if x,y are of different signs, red if they are the same. This is not doable using a linear classification model.

The idea behind kernel methods is to project the linearly inseparable data into a higher-dimensional data to convert it to linearly separable data. Here's an example:

![[Pasted image 20260216134459.png]]

We see that the original dataset could only be separated using a circle, but by projecting it we can separate using a hyperplane which we find by training a linear SVM model in the new feature space.

The main problem is that constructing the new feature space is computationally expensive, and computing the dot product is hard. Instead of trying to figure out what projection mapping to use on the data, we can just apply a kernel function on it. The most popular one is the RBF/Gaussian kernel which interprets the similarity between a pair of examples.

The gamma parameter in the SVM, when we increase it, will lead to a tighter decision boundary. This could definitely lead to overfitting so be careful about this.

## Decision trees

Decision trees are good models if we care about interpreting how to classify the data. This model breaks data down by making a decision based on asking a series of questions. For example, a question might look like "Is the sepal width >= 2.8 cm".

We start at the tree root. The data is then split on the feature that results in the largest information gain. Then, repeat until a certain maximum depth (this is called pruning and prevents overfitting). 

We define the information gain function as
$$IG(D_p, f) = I(D_p) - \sum_{j=1}^m\frac{N_j}{N_p}I(D_j)$$
Simply put, the information gain is the difference between the impurity of the parent node and the sum of the child node impurities. If the child node are much more pure and close in classification, it means we gained a lot of meaningful classification information.

For simplicity, scikit-learn implements binary decision trees, where each parent node is split into two child nodes.

There are three different impurity measures which are commonly used: Gini impurity, entropy, and classification error. The entropy is defined as $$I_H(t) = -\sum_{i=1}^c p(i|t)log_2p(i|t)$$
Here $p(i|t)$ represents the proportion of examples belonging in class i. The entropy is 0 if all examples are in the same class, and maximal if the classes are uniformly distributed.

The Gini impurity is a criterion to minimize the probability of misclassification: $$I_G(t) = 1 - \sum_{i=1}^c p(i|t)^2$$
Similar to entropy, if the classes are perfectly mixed, it is maximal. In practice, the Gini impurity and the entropy lead to similar results. These two are the ones used practically for growing decision trees.

The misclassification error is $$I_E(t) = 1 - \max\{p(i|t)\}$$, but this is not commonly used as it is less sensitive to change in class probabilities of the nodes.

In practice, running a decision tree on a numerical dataset involves splitting the feature space into rectangles. Note that feature scaling is not required for decision tree algorithms.

One nice feature in scikit-learn is that it allows us to visualize the decision tree model after training, by using tree.plot_tree(). It will show us how the data branches at each step.

There is an algorithm known as random forest, which is essentially a collection of decision trees. The idea behind using an ensemble of trees is that by taking the average, you minimize the variance(overfitting). Here are the steps:

- Randomly choose a bootstrap sample of size n
- Grow a decision tree from the bootstrap sample. Randomly select some features to consider when growing the tree, rather than all.
- Repeat the above 2 steps many times
- Aggregate the predictions together

There is a lower level of interpretability but we don't need to worry too much about choosing good hyperparameter values.

## K-nearest neighbors

The previous algorithms we saw have a specifical mathematical function to separate classes using weights and biases, and learn these parameters during training. In contrast, KNN memorizes the training dataset and will only do work when we request a prediction. That's why it's called a lazy learner.

Here is the algorithm:

- Choose the number k and a distance metric
- We have a data record we want to classify. Find the k-nearest neighbors and their classes.
- Assign the class label by majority vote

The advantage of a memory-based approach such as KNN is that the classifier immediately adapts as we collect new training data. The downside is that the complexity for classifying new examples grows linearly. For small/medium-sized datasets, memory-based methods can provide good performance.

To find the balance between overfitting and underfitting, choose the right k is important. Choosing the right distance metric is also important. For example, we could pick Euclidean, or minkowski. Make sure to standardize the data so each feature contributes equally to the distance.

The main problem with KNN is that it is very susceptible to overfitting due to the curse of dimensionality. The idea is if you have a lot of features in the dataset, distance between points might not mean anything anymore.
