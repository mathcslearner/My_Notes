We can often summarize the information in a dataset by transforming it onto a new feature subspace of lower dimensionality. This is known as data compression and is very useful to save money and time. Here are the three main ones covered:

- Unsupervised data compression using PCA (principal component analysis)
- Supervised dimensionality reduction using linear discriminant analysis
- Nonlinear dimensionality reduction

## Unsupervised

The goal is to maintain most of the relevant information while improving computational efficiency. It also allows us to reduce the curse of dimensionality.

PCA (principal component analysis) identifies patterns in the data based on correlation between features. It finds the directions of maximum variance (given the constraint that these directions must be orthogonal), and uses those as the new feature axes for the new subspace.

Let's say we want to map a vector from a d-dimensional feature space to a k-dimensional feature subspace. Then the transformation matrix would be of dimension d x k, and we would transform $x$ by doing $xW = z$, where $z$ is the output vector.

Note that we need to standardize the features so that they all contribute equal importance.

Here's the summarized approach for how to do PCA:

- Standardize the d-dimensional dataset.
- Construct the covariance matrix.
- Decompose the covariance matrix into eigenvectors, eigenvalues.
- Sort the eigenvalues by decreasing order to rank the eigenvectors
- Select the k eigenvectors corresponding to the k largest eigenvalues
- Construct a projection matrix W from those k eigenvectors
- Transform the d-dimensional input dataset, X, using the projection matrix W to obtain the new k-dimensional feature space

(A note on the math: the covariance matrix will always be symmetric by construction. Then, it is well-known from linear algebra that all eigenvalues are real and the eigenvectors are orthogonal to each other.)

As a reminder, for features $j$ and $k$, the covariance between $x_j$ and $x_k$ is $$\sigma_{jk} = \frac{1}{n-1} \sum_{i=1}^n (x_j^{(i)} - \mu_j)(x_k^{(i)} - \mu_k)$$
Here $\mu_j, \mu_k$ are the sample means of features j and k, which would be zero if we standardized the dataset. A positive covariance between two features indicates that the features have strong positive correlation, while a negative would indicate strong negative correlation.

Then the correlation matrix would look like this (example for d = 3):

![[Pasted image 20260222221138.png]]

In numpy, we can compute covariance matrix using numpy.cov, and we can use linalg.eig to obtain the eigenpairs.

To see how many eigenvectors to pick, we can consider picking as many as it takes to reach a certain "information threshold". We can do so by computing the explained variance ratio of an eigenvalue, which is the information it provides. It is simply $$\text{Explained variance ratio} = \frac{\lambda_j}{\sum_{j=1}^d \lambda_j}$$
Then, we can check the cumulative sum of explained variances to see how much variance we capture using the first k eigenvectors.

To use PCA in practice, we first use it to bring data down into a lower-dimensional feature space, then we can train the logistic regression model on it, or any other model. 

One interesting thing we could do is look at how each original feature contributes to a given principal components. These contributions are known as loadings. We can compute these by doing eigenvec \* sqrt(eigenval).

## Supervised

The main technique used is called linear discriminant analysis (LDA). The general goal is to find the feature subspace that optimizes class separability.

The assumptions used in LDA are that the data is normally distributed, classes have identical covariance matrices and training examples are independent of each other. However, LDA is still reasonable even when the assumptions are not perfectly satisfied.

Here are the main steps to reduce from d dimensions to k dimensions:

- Standardize the d-dimensional dataset
- For each class, compute the d-dimensional mean vector
- Construct the between-class scatter matrix $S_b$ and the within-class scatter matrix $S_w$
- Compute the eigenvectors/eigenvalues of $S_w^{-1}S_B$ 
- Sort the eigenvalues by decreasing order to rank the corresponding eigenvectors
- Choose the k eigenvectors which largest eigenvalues to construct a d x k transformation matrix $W$ using the eigenvectors as columns
- Project the training examples onto the new feature subspace using $W$

We can see that this is pretty similar to PCA, except we use a different matrix (since we are using a different criterion). The main difference is that LDA takes class label information into account (mean vectors in step 2).

The mean vectors with respect to class i is $$m_i = \frac{1}{n_i}\sum_{x \in D_i}x_m$$
Then we can compute the within-class scatter matrix as $$S_W = \sum_{i=1}^c S_i$$
where $$S_i = \sum_{x \in D_i} (x - m_i)(x-m_i)^T$$ is the scatter matrix of the individual class i.

The assumption is that the class labels in the training dataset are uniformly distributed. If not, then we need to scale the individual scatter matrices $S_i$ before summing them up. To scale them, just divide by the number of class examples $n_i$. In fact this turns out to be the covariance matrix $$\Sigma_i = \frac{1}{n_i}S_i = \frac{1}{n_i} \sum_{x \in D_i} (x-m_i)(x-m_i)^T$$
Then, to calculate the between-class scatter matrix, we can do $$S_B = \sum_{i=1}^c n_i(m_i - m)(m_i-m)^T$$ where $m$ is the overall mean of the entire dataset.

When performing LDA, the number of linear discriminants is at most $c-1$ where $c$ is the number of class labels, so only $c-1$ eigenvalues will be nonzero.

Similar to PCA, we can compute how much class-discriminatory information is captured by an eigenvector by computing a ratio of eigenvalues.
## Nonlinear

One nonlinear dimensionality reduction technique that will be seen in this section is t-distributed stochastic neighbor embedding (t-SNE), which is frequently used to visualize high-dimensional datasets in 2/3 dimensions.

If we encounter nonlinear problems where training data is not linearly separable, linear transformation techniques such as PCA and LDA might not work the best. The main advantage of PCA/LDA is that they are simpler to understand and we can easily visualize the efficiency of the results.

The basic idea of t-SNE is that it checks the pair-wise distances between the data points. Then it finds a probability distribution of pair-wise distances in the lower-dimensional space which is a good approximation of the original distances. Do note that t-SNE is only usable on the whole dataset as a visualization tool, and cannot be applied to new data points.

Just like PCA, t-SNE is an unsupervised method.

Another popular visualization technique is uniform manifold approximation and projection (UMAP), which typically runs faster.
