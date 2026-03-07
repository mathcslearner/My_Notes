# Model Evaluation and Tuning

Previously we learned that when we apply preprocessing techniques on the training dataset, we have to also apply them separately on the test dataset. The Pipeline class in sklearn is very useful for this.

For example, instead of doing a model fitting and then a data transformation/PCA step for the training and test datasets separately, we can chain StandardScaler, PCA, and LogisticRegression objects from sklearn together in a pipeline, and directly apply it to the datasets we have.

The make_pipeline function takes any number of transformers, then an estimator at the end (for the prediction task). Below is a figure which summarizes pretty well how a sklearn pipeline works:

![[Pasted image 20260305211640.png]]

## Cross-validation

Cross-validation techniques allow us to understand how well the model can generalize to unseen data.

One such method is the holdout method. The idea is to separate the data into three parts: training, validation, and test. We come up with a few different models with different parameters, since we want to pick a model which has optimal values of hyperparameters. The training dataset is used to fit the different models, then we check which one performs best on unseen data using the validation dataset. Finally we can test the model's results on the test dataset. One disadvantage of this method is that the performance estimate can be sensitive to how the different datasets are partitioned.

Another method is the k-fold cross-validation:

- First, we randomly split the training dataset into k folds. k-1 of these are training folds, and the last one is the test fold. Then we shift the folds by one, so that another fold is used as a test fold, etc.
- For each model, we can calculate its average performance based on the independent test folds. This yields a performance estimate less sensitive to how the training data is partitioned, and allows us to find better hyperparameter values.
- After we have found good hyperparameter values, we can retrain the model on complete training dataset and obtain a final performance estimate using the independent test dataset.

Below is a figure visualizing how k-fold cross-validation looks like:

![[Pasted image 20260305212909.png]]

Empirical evidence shows that k=10 yields the best bias-variance tradeoff in general. If we are working with small training sets, it can be useful to increase the number of folds (as it won't be that expensive computationally).

A variant of this method is the stratified k-fold cross-validation, where each fold is representative of the class proportions in the training dataset. We can call it using StratifiedKFold in sklearn.

## Diagnostic tools

Here are two diagnostic tools that can help us improve the performance of a learning algorithm: learning curves and validation curves.

Learning curves can be use to detect whether a model suffers from high variance or high bias. Here's a visualization of how learning curves might look like for models with issues:

![[Pasted image 20260305215315.png]]

For models with high bias, common solutions are to increase the number of model parameters, or decrease the degree of regularization. Basically, your results are bad because your model is not complex enough to capture the data.

For models with high variance, there is overfitting as there is a large gap between the training and validation accuracy. To reduce this problem, we can collect more data, reduce the complexity, or increase the regularization parameter.

For unregularized models, it can help to decrease the number of features.

Sklearn comes with a learning curve function which allows us to visualize the training accuracy vs the validation accuracy. By default, the validation accuracy of a classifier is calculated using stratified k-fold cross-validation.

Validation curves, meanwhile, are used to improve the performance of a model by addressing overfitting/underfitting. The idea behind validation curves is that we vary the values of the model parameters and check the test vs train accuracy for a varied range of parameters. For example, we could vary the inverse regularization parameter, C.

## Hyperparameter tuning

Another popular hyperparameter optimization technique is known as grid search. The idea is pretty simple, we specify a list of valid values for different hyperparameters, and the computer evalues the model for each combination, to obtain the optimal combination of values. Visually it looks like a grid. GridSearchCV also uses k-fold cross validation.

The problem with grid search is that specifying large parameter grids makes the search very expensive in practice. An alternative approach is randomized search, which allows us to explore a wide range of values in a more cost-efficient manner. Since grid search is discrete, it might miss good hyperparameter configurations if we don't have a good enough search space. Here's a figure visualization:

![[Pasted image 20260306211727.png]]

It is possible to make randomized search even more efficient by implementing successive halving. Here's the basic algorithm:

- Draw a large set of candidate configurations using random sampling
- Train the models with limited resources, for example a small subset of the training data
- Discard the bottom 50 percent
- Repeat step2-3 until we are left with one configuration

Suppose we want to select among different machine learning algorithms: we can use nested cross-validation. The idea is we have an outer k-fold cross-validation loop which splits the data into training and test folds, and then an inner loop which selects the model using k-fold cross-validation on the training fold. Here's an example of a 5x2 cross-validation:

![[Pasted image 20260306212951.png]]

We can run nested cross-validation alongside grid search or other similar techniques. The idea is to run nested cross-validation on the different models to verify which one generalizes the best for unseen data.

## Performance Evaluation Metrics

Previously we mainly evaluated different ML models using the prediction accuracy. There are other performance metrics which can also be used to measure a model's relevance.

First, note that to visualize the performance of a learning algorithm, we can use a confusion matrix. A confusion matrix looks like this:

![[Pasted image 20260306214123.png]]

The green squares are correctly predicted and the red squares are incorrectly predicted. From the confusion matrix above, we can calculate the prediction error $$\text{ERR} = \frac{\text{FP} +\text{FN}}{\text{FP} +\text{FN} + \text{TP} + \text{TN}}$$
and the prediction accuracy $$\text{ACC} = \frac{\text{TP} +\text{TN}}{\text{FP} +\text{FN} + \text{TP} + \text{TN}} = 1 - \text{ERR}$$
The true positive rate is $$\text{TPR} = \frac{\text{TP}}{\text{TP}+\text{FN}}$$ and the false negative rate is  $$\text{FPR} = \frac{\text{FP}}{\text{FP} + \text{TN}}$$
These are metrics which are useful for imbalanced class problems. For example, in tumor diagnosis, the FPR would be useful as we do not want to misdiagnose patients.

Here are two more performance metrics: precision is $$\text{PRE} = \frac{\text{TP}}{\text{TP} + \text{FP}}$$ and recall is the same as TPR. 

Let's look at the tumor diagnosis example again. If we optimize for recall, we minimize chances of not detecting a malignant tumor, in exchange of a high number of FPs. If we optimize for precision, then we won't have many FPs, but at the cost of a high number of FNs.

To balance these two, we can use the F1 score defined as $$\text{F1} = 2 \frac{\text{PRE} \times \text{REC}}{\text{PRE} + \text{REC}}$$
Another measure is the MCC, which is calculated as $$\text{MCC} = \frac{\text{TP}\times\text{TN} - \text{FP}\times\text{FN}}{\sqrt{(\text{TP} + \text{FP})(\text{TP}+\text{FN})(\text{TN}+\text{FP})(\text{TN}+\text{FN})}}$$
This will always range between -1 and 1, and takes all elements of a confusion matrix into account (F1 does not include TN). This MCC score is harder to interpret but it is a superior metric.

All the scoring metrics from above are in sklearn. In GridSearchCV, we can actually choose the scoring metric we want to use when doing hyperparameter optimization.

A tool we can use to select models for classification based on their performance with respect to the FPR and TPR is the receiver operating characteristic (ROC) graphs. The diagonal represents random guessing. We can compute the ROC area under the curve to see how good the performance is. Similarly, we can also compute precision-recall curves. Do note that the ROC area under curve and accuracy metrics mostly agree with each other.

The scoring metrics seen previously are specifically for binary classification systems. It is possible to extend those scoring metrics to multiclass problems via one-vs-all classification.

## Class Imbalance

Consider an example where our dataset is comprised of 90% class1 and 10% class2. Then, achieving a 90% accuracy result doesn't mean much, as we could just predict class1 for everything. Therefore, we should use other metrics than accuracy. For example, precision, recall, ROC curve, etc.

Another problem with class imbalance is that during the model fitting phase, the model will naturally be biased towards the majority class. One way to deal with that is to assign a larger penalty to wrong predictions on the minority class. Another strategy is to upsample the minority class.
