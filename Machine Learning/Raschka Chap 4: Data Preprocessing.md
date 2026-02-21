Before training a machine learning model, it's important to examine and preprocess the dataset. Here are 3 components to preprocessing:

- Removing and imputing missing values from the dataset
- Getting categorical data into shape
- Selecting relevant features for model construction

## Missing data

In real world data, missing values are normal. Typically they will be either blank or NaN or NULL(common in relational databases). Here's a rough workflow for how to deal with missing values:

- Identify missing values
- Eliminate training examples/features with missing values or imputing missing values

To check how many missing values there are per column, we can run df.isnull().sum()

To remove the columns/rows with missing values, we can use df.dropna(axis=0) and df.dropna(axis=1). To delete rows where all columns are missing, we can use df.dropna(how="all"). There are many different arguments to df.dropna to specify how to delete certain row/columns.

The main problem with removing missing data is that we might remove too many samples, or too many feature columns. In this way we lose valuable data.

The alternative is to use interpolation techniques to estimate the missing values. One strategy is to replace missing values with the mean of the feature column. We can do this using SimpleImputer class from sklearn. We can also use median, most_frequent, etc.

An alternative is to use pandas' fillna(). For ex, for mean imputation we can directly do df.fillna(df.mean()).

## Categorical data

There are two kinds of categorical data:

- Ordinal: Can be ordered, for ex. Tshirt size would be XL > L > M
- Nominal: No order implied, for ex. Tshirt color

For ordinal features, to make sure learning algorithm interprets them correctly, we should convert them to integers, for example XL->3, L->2, M->1. We have to do this manually. Later on, if we want to transform the integer values back to the string representation, we can use a reverse dictionary.

Class labels are usually required to be encoded as integer values. It doesn't matter which one is which, as long as they are different. We can do so using a mapping. Alternatively we can use the LabelEncoder class in sklearn to directly convert labels to values.

For nominal features, we could also map them to integers. For example blue=0, green=1, red=2. The problem is that now this implies an order between the features, which will get learned in the model.

The workaround is known as one-hot encoding. The idea is to create a new feature for each unique value, for example we can create a feature named blue, one named green, one named red. Then the values would be binary. We can do this using oneHotEncoder from sklearn or get_dummies from pandas.

One issue with one-hot encoding is that this introduces multi-collinearity(high column correlation), which can be an issue for methods which require matrix inversion. The solution would be to simply drop one feature column, for ex. drop color_blue.

## Preprocessing wine dataset

The first step is to separate the data into separate training and test datasets. To partition dataset into training and test data, just use sklearn's train_test_split function. The most commonly used test splits are 60/40, 70/30 or 80/20. For extremely large datasets, it would be fine to leave most examples for training.

It is also possible to retrain the classifier on the entire dataset after model training and evaluation, to improve the performance.

The next step is to bring features into the same scale, since most algorithms require this (we want features to affect the error equally). There are two ways: normalization (which brings data to 0-1) and standardization (which fits the data into a bell curve centered at 0 with std dev 1).

One of the main reasons for selecting meaningful features is to combat against overfitting (high variance, overly complex learned models). 

As seen previously, one way to reduce model complexity is by penalizing large individual weights using a regularization factor. We have either L2 regularization $$||w||_2^2 = \sum_{j=1}^m w_j^2$$ or L1 regularization $$||w||_1 = \sum_{j=1}^m |w_j|$$
L1 regularization yields sparse feature vectors where most weights will be zero. This is useful to get rid of irrelevant dimensions and can be treated as a means for feature selection.

To access the weight and bias units, we can use lr.intercept_ and and lr.coef_. If we are training logistic regression on a multiclass dataset via OvR approach, we will get many different models (one for class 1 vs not class 1, one for class 2 vs not class 2, etc).

As a reminder, the regularization strength is controlled with the value of C in the API call for logistic regression. The smaller C is, the larger the regularization.

As said previously, another way to reduce the complexity is via feature selection, where we basically select a subset of features to use. Sequential feature selection algorithms are greedy search algorithms used to reduce the dimension of the feature space.

One such algorithm is the sequential backward selection (SBS) which aims to reduce dimensionality of feature space with minimum decay in performance. Here is the basic algorithm:

- Initialize with k=d where d is the dimension of the full feature space $X_d$
- Determine the feature $x^*$ that minimizes the criterion $J(X_k - x)$ where $J$ represents the performance loss after removal and $x \in X_k$
- Remove the feature $x^*$ from the feature set: $X_{k-1} = X_k - x^*$ and $k = k-1$
- Terminate if we reach the number of desired features, otherwise go to step 2 and repeat

Basically, at each step, we eliminate the feature that causes the least performance loss after removing that feature.

Note that when we use SBS, only give it the training dataset, not the test dataset.

Another approach for selecting the most important features from a dataset is using a random forest. We can measure the feature importance as the averaged impurity decrease from all decision trees.

When using RandomForestClassifier in sklearn, it already collects the feature importance values for us, which is nice. Note that for tree-based models, we do not need to have standardized or normalized features.

One issue with this random forest approach is that if two features are highly correlated, one might be ranked very high while the other would be low. This would lead to problems trying to interpret the feature importance values.
