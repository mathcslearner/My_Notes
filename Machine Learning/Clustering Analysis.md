# Clustering Analysis

Cluster analysis is a category of unsupervised learning techniques that allows us to discover hidden structures/groups in data, in a context where we do not already have predefined classes. Elements in the same cluster are more similar than those in different clusters.

## Prototype-based clustering

One of the most popular algorithms is k-means, which is very computationally efficient. k-means is prototype-based clustering, so each cluster is represented by a prototype, usually the centroid. It is very good at identifying clusters with a spherical shape. The disadvantage is that we must choose k in advance, and the wrong choice can result in poor clustering performance.

Here is the algorithm itself:

- Randomly pick k centroids from the examples as initial cluster centers
- Assign each example to nearest centroid
- Move the centroids to the center of the examples that were assigned to it
- Repeat step2, 3 until the assignment stabilizes or a maximum number of iterations is reached

To define similarity, we can simply use the squared Euclidean distance. The goal with k-means is to minimize the sum of squared errors SSE, defined as $$\text{SSE} = \sum_{i=1}^n \sum_{j=1}^k w^{(i,j)} ||x^{(i)} - \mu^{(j)}||^2$$
Here $\mu^(j)$ represents the jth centroid and $w^{(i,j)}$ is an indicator variable for whether the input $x^(i)$ is in the cluster j. The SSE represents the sums of the distances between each point and its corresponding centroid, which makes sense.

Technically, another issue with k-means is that clusters could potentially be empty, but the sklearn implementation fixes this.

The fact that the algorithm uses a random seed to place the initial centroids can result in bad clusterings or slow convergence. One strategy is to run it multiple times and choose the model with the best SSE.

Another one is to place the initial centroids far away from each other. This is know as k-means++. The basic idea of the algorithm is just to be more likely to pick centroids which are far away from the remaining points, using a probability distribution based on distance.

In the above described k-means, every example is assigned to exactly one cluster. This is called hard clustering. Another way to do this is to assign examples to one or more. This is known as soft/fuzzy clustering.

One such algorithm is the fuzzy C-means (FCM) algorithm. Previously, we would have $w^{(i, j)} = 1$ for one $j$ and $w^{(i, j)} = 0$ for all other $j$. Now, we can assign these values as a probability distribution, so the sum of $w^{(i, j)}$ over $j$ is 1. The value would represent the probability of membership to this cluster. Then we can proceed like the normal k-means algorithm.

As mentioned previously, one of the main challenges is finding the right number of clusters. We can use the elbow method for that. For a given k, we can calculate the SSE of using k clusters. As k increases, the SSE will decrease, but we don't want k too large as we lose information. We want the value of k where the distortion begins to increase most rapidly. Visually the point we want looks like an elbow:

![[Pasted image 20260331171350.png]]

Another metric we can use to evaluate the quality of clustering is silhouette analysis. It is a graphical tool to measure how tightly grouped the clusters are. For a single example, the silhouette coefficient is calculated as:

- The cluster cohesion $a^{(i)}$ is the average distance between the example $x^{(i)}$ and all other points
- The cluster separation $b^{(i)}$ is the average distance between the example $x^{(i)}$ and all examples in the next closest cluster
- The silhouette $s^{(i)} = \frac{b^{(i)} - a^{(i)}}{\max{b^{(i)}, a^{(i)}}}$

This coefficient is bounded between -1 and 1. The coefficient is 0 if the cluster separation and cohesion are equal. We get close to 1 if our example is much closer to its own cluster than the next closer cluster.

A good clustering would have a silhouette plot where the clusters are all approximately equally far away from the average, and the coefficients are not close to 0. Around 0.7 is fine. If they have visibly different lengths and widths, it means the clustering is suboptimal.

## Hierarchical clustering

Another approach to clustering is called hierarchical clustering. It allows us to plot dendrograms, which helps us interpret the results by creating meaningful taxonomies. One advantage is we don't have to provide a number of clusters.

A main approach to hierarchical clustering is agglomerative clustering. Every example is initially a single cluster, and we merge closest pairs of clusters iteratively. Here's a basic algorithm for complete linkage clustering:

- Compute pair-wise distance matrix of all examples
- Represent each data point as a singleton cluster
- Merge two closest clusters, based on the longest pairwise distance between them
- Update the cluster linkage matrix
- Repeat until we have one cluster left

Using a dendrogram we can visualize which clusters are closest to each other. It is based on which pair of clusters were chosen to be merged at every iteration. We can also combine dendrograms with heat maps, which represent the individual values in the matrix. This allows us to further visualize which rows are most similar to each other.

Using sklearn's AgglomerativeClustering class, we can also choose how many clusters we want to return (the default being 2).

## Density-based clustering

A final approach to clustering is density-based spatial clustering (DBSCAN). This method assigns cluster labels based on dense regions of points. Here's the basic algorithm:

- First, each example is given a label.
- A point is a core point if at least MinPts of neighboring points fall within the specified radius $\epsilon$
- A point is a border point if it has fewer neighbors than MinPts within $\epsilon$ but lies with $\epsilon$ of a core point
- Otherwise a point is a noise point
- Then, form a separate cluster for each core point/connected group of core points (they are connected if $\leq \epsilon$ away)
- Assign each border point to the cluster of its corresponding core point

Here's a visual representation:

![[Pasted image 20260331175845.png]]

One of the main advantages of DBSCAN is that it can cluster data of arbitrary shapes, something the other methods struggle with. The main disadvantage is the curse of dimensionality. We also need to optimize the two hyperparameters (MinPts and $\epsilon$) to obtain good results.

To reduce the impact of the curse of dimensionality, it is common practice to apply dimensionality reduction techniques, such as PCA and t-SNE. We can also visualize the clusters better by compressing datasets down to two-dimensional scatterplots.
