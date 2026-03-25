# Ensemble Learning

The main goal of ensemble methods is combining different classifiers such that the resulting model has better generalization performance than the individual models.

One thing to note about ensemble techniques is that it increases computational complexity, so in practice we need to think about the price-performance tradeoff.
## Majority Voting Principle

The most popular ensemble methods use the majority voting principle, so we select the class label which has been predicted by the majority of classifiers. If there are multiple classes, then we can proceed by a plurality vote, where we pick the label which received the most votes.

We start by training different classifiers on the training dataset. These classifiers can come from different classification algorithms, such as decision trees, support vector machines, logistic regression, etc.

Using combinatorics we can show that the error rate of an ensemble is much lower than the error rate of each individual classifier, which makes sense intuitively since the prediction has to be made by many different classifiers. 

When we plot error rate of ensemble vs error rate of individual, we can see that whenever the base classifiers have base error less than 0.5 (i.e. they are better than random guessing), the ensemble classifier will always perform better.

When implementing a majority vote classifier, note that we can also assign weights to each classifier, so certain classifiers could be held in more importance for our final prediction. To simulate the weighting, let's say a prediction made by a certain classifier has three times more weight, we can just repeat the result of that classifier three times when we do the final calculation of which prediction to take.

We could also use predicted class probabilities, where we just take the label which has the highest weighted probability. The predicted class label can be written as $$y = \arg \max_i \sum_{j=1}^m w_jp_{ij}$$
where $i$ represents the class labels, $w_j$ are the weights for the classifiers and $p_{ij}$ is the predicted probability of the jth classifier for the ith label.

## Bagging

Bagging is another form of ensemble classifiers. The main idea is that we draw bootstrap samples (random with replacement) from the initial training dataset when we train the individual classifiers, rather than just using the same training dataset. To make the final prediction, we still use majority voting. Here's a picture visualizing how it works:

![[Pasted image 20260321112917.png]]

We can view bagging as a sort of generalization of random forests, since they both use a random subset of the training dataset for each different classifier.

To use bagging, we can call the BaggingClassifier class from sklearn. Usually, for bagging models, we will use an ensemble of decision trees.

The main strength of the bagging algorithm is that it allows us to reduce overfitting from single decision trees, and it reduces the variance of a model. Do note that bagging is not good in reducing model bias. That is why we usually perform bagging on classifiers with low bias, such as unpruned decision trees.
## Adaptive Boosting

In boosting, we use very simple base classifiers (called weak learners), which have only a slight performance advantage over random guessing. An example of a weak learner would be a decision tree stump. The main idea behind boosting is to let the weak learners learn from misclassified training examples to improve the performance of the ensemble.

Here's the basic algorithm for the original boosting procedure:

- Draw a random subset of training examples $d_1$ from the training dataset $D$ without replacement to train weak learner $C_1$
- Draw another random subset of training examples $d_2$ without replacement, add 50 percent of the misclassified examples, then train weak learner $C_2$
- Find the training example in $D$ which $C_1$ and $C_2$ disagree upon, to train a weak learner $C_3$
- Combine $C_1$, $C_2$, $C_3$ via majority voting

Note that in practice, boosting algorithms can have high variance, so they overfit the training data.

AdaBoost uses the complete training dataset to train the weak learners, and the training examples are reweighted every iteration so that the classifier learns from the mistakes of the previous weak learners. We assign a larger weight to misclassified examples and smaller weight to correctly classified examples, so that the next weak learner is more focused on the harder examples. Here's a visualization:

![[Pasted image 20260321120008.png]]

Here's a more detailed mathematical view of the algorithm ($\times$ denotes component-wise multiplication and $\cdot$ denotes dot product):

- Set the weight vector $w$ to uniform weights where $\sum_i w_i = 1$
- For j in m boosting rounds, do:
	- Train a weighted weak learner $C_j = \text{train}(X, y, w)$
	- Predict class labels $\hat{y} = \text{predict}(C_j, x)$
	- Compute weighted error rate $\epsilon = w \cdot (\hat{y} \neq y)$
	- Compute the coefficient $\alpha_j = 0.5 \log{\frac{1 - \epsilon}{\epsilon}}$
	- Update the weights $w = w \times \exp( -\alpha_j \times \hat{y} \times y)$
	- Normalize the weights so that they sum to $1$, by doing $w = \frac{w}{\sum_i w_i}$

To use adaptive boosting we can just call AdaBoostClassifier in sklearn.

## Gradient boosting

Gradient boosting forms the basis of popular machine learning algorithms like XGBoost.

Here's the basic outline of the general gradient boosting algorithm for classification. The main idea is to build a series of trees, where each tree is fit on the error of the previous tree. Then, in each round, the tree ensemble improves as we update them based on a loss gradient (hence the name).

![[Pasted image 20260325154248.png]]

For the loss function, we can use the logistic loss function from logistic regression. Then, for classification specifically, here are the steps for the algorithm:

![[Pasted image 20260325154443.png]]

Selecting the parameters for XGBoost is important. For the learning rate, between 0.01 and 0.1 is recommended. For the max_depth for the individual decision trees, between 2 to 6 is usually fine.
