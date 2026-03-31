# Regression Analysis

Regression analysis is another subcategory of supervised learning. The idea is to predict target variables on a continuous scale rather than a categorical class label.

One method is linear regression. We want to model the relationship between features and a continuous target variable using a linear function.

The easiest is a simple/univariate linear regression. We want to model the relationship between a feature $x$ and a target $y$ using a linear equation $y = w_1x+b$. The goal is to learn the weights $w_1$ and $b$. Then, we can use the equation to predict $y$ for $x$ which are not in the training dataset.

To learn the weights, the idea is to find the best-fitting straight line (regression line) through the training examples which minimizes the errors (residuals). Here's a visual:

![[Pasted image 20260330155351.png]]

For multiple linear regression, we can generalize the equation as $y = w_1x_1 + \dots + w_mx_m + b = w^T x + b$ . Note that training a linear regression model does not require the variables be normally dsitributed.

One useful thing we can do before training the model is to do some exploratory data analysis to get a sense of the data. This allows us to detect the presence of outliers, the data distribution, and the relationships between features. Here are some useful things to do:

- Scatterplot matrix for pairwise correlations between features
- Quantify linear relationships between variables using a correlation matrix

A score of r = 1 indicates perfect positive correlation, r = 0 means no correlation, r = -1 means perfect negative correlation. Using the results from EDA, we can use the most correlated features in order to run a simple linear regression model.

To estimate the parameters of the linear equation, we can use the ordinary least squares (OLS) method. Basically, we want to minimize the sum of the squared residuals of the training examples. We can do so using gradient descent, just like for Adaline (in fact it's the same loss function we are trying to minimize). For better convergence, we should standardize the features. Don't forget to scale the target variable back to the original scale using an inverse transform when reporting.

Do note that for sklearn LinearRegression class, we don't need to standardize variables.

One disadvantage of linear regression is that such models can be heavily impacted by the presence of outliers. To deal with this without simply throwing out outliers, we can look at the Random Sample Consensus (RANSAC) algorithm. The basic idea is to:

- Select random number of examples to be inliers and fit the model
- Test all other data points against the fitted model, add points that fall within a certain tolerance to the inliers
- Refit the model using all inliers
- Estimate error of fitted model versus inliers
- Terminate algorithm if performance meets a certain threshold, restart otherwise.

As for all other models, it is crucial to test the model on data it hasn't seen to understand its generalization performance.

For multiple regression model, it's hard to visualize the hyperplane. However we can plot the residuals between the actual and predicted values, and that can help us diagnose our regression model.

For a good regression model, the errors should be randomly distributed and the residuals randomly scattered around the centerline. If there are patterns, it means the model wasn't able to capture some explanatory information.

Another way to measure performance is the MSE, $$\text{MSE} = \frac{1}{n}\sum_{i=1}^n (y^{(i)} - \hat{y}^{(i)})^2$$
Another metric is the MAE (mean absolute error), $$\text{MAE} = \frac{1}{n}\sum_{i=1}^n |y^{(i)} - \hat{y}^{(i)}|$$, which emphasizes incorrect prediction slightly less and can show the error on the original scale.

One problem with the MSE/MAE is that they are not standardized, so a model operating with quantities which are lower can have lower MSE/MAE despite having higher standardized error. We can also compute the $R^2$ score, which is a rescaled version of the MSE. If it is negative, it means the model fits the data worse than simply taking the sample mean. If it is 1 then it fits perfectly.

To tackle overfitting, we can use regularization, which penalizes complexity. Some approaches for regularized linear regression are ridge regression, LASSO and elastic net. The main idea is to add a term depending on the magnitude of the weights to the loss function.

It's also possible our model is not linear but rather polynomial, that is $y = w_1x + w_2x^2 + \dots + w_dx^d + b$. We can also do this regression using sklearn. As always, be careful because adding more polynomial features increases the complexity of a model, and thus the chance of overfitting.

We can also do random forest regression. A random forest can be viewed as a sum of piecewise linear functions. An advantage is that it works with arbitrary features and data does not need to be transformed.

Recall that a decision tree splits to maximize information gain / minimize impurity. Here we can define the impurity as the MSE, so it will minimize that. The problem with a decision tree regression is that it does not capture continuity and differentiability of the prediction, since it splits abruptly. It can capture general trends well though.

These disadvantages can somewhat be overcome with a random forest.
