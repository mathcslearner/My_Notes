# Sentiment Analysis

In today's Internet era, people's opinions and reviews are a valuable resource for data analytics and machine learning. Sentiment analysis is a subfield which allows us to classify documents based on the sentiment of the writer (positive, negative, etc.).

The basic workflow of the sentiment analysis model we will build is:

- Cleaning and preparing text data
- Building feature vectors from text documents
- Training an ML model to classify positive and negative movie reviews
- Working with large text datasets using out-of-core learning
- Inferring topics from document collections for categorization

This model will be based on movie reviews from IMDb, which are labeled as either positive or negative.

For a machine learning algorithm, we cannot directly feed it text; we have to convert the data into a numerical form first. The bag-of-words model allows us to represent text as numerical feature vectors. The idea is

- Create a vocabulary of unique tokens from all the documents
- Construct a feature vector from each document, containing the count of how often each word occurs

Since each document will contain only a small subset of all the words, the feature vectors will be sparse. Note that the word or term order does not matter for the feature vectors (the indices are usually assigned alphabetically anyways). We can use the CountVectorizer class in sklearn.

Some common words will occur across multiple documents from both classes, yet not carry any discriminatory information. Using term frequency-inverse document frequency (tf-idf), we can "downweight" these common words. The tf-idf is defined as $$\text{tf-idf}(t, d) = \text{tf}(t, d) \times \text{idf}(t, d)$$ where tf(t, d) is the term frequency of term t in document d, and idf(t, d) is the inverse document frequency, calculated as $$\text{idf}(t, d) = \log\frac{n_d}{1+\text{df}(d, t)}$$
Note that $\text{df}(d, t)$ represents the number of documents d which contain t. The idea is that if a lot of documents contain this word then it should be weighted less. 

In sklearn we have the TfidfTransformer class. This class will automatically normalize the resulting feature vectors.

When doing text processing, before using bag-of-words, term frequencies, etc, we must first clean the text data by stripping all the unwanted characters. For example, some of the movie reviews contain HTML markup, and other non-letter characters. We should also convert the text into lowercase characters so that the counts are accurate. We can do so using regular expressions. 

After cleaning the dataset, we should split the text corpora into individual elements through tokenization. One way to do so is to just split into individual words. Another way is to do word stemming, so transforming a word into its root form. One such algorithm is the Porter stemmer algorithm. We can also run stop word removal, which basically removes the most common words which do not affect the meaning (is, has, and, etc.) You can do this as an alternative to tf-idf.

After doing text processing and we have nice clean feature vectors, we can just run logistic regression model as usual, for classification tasks. We can also add grid search for hyperparameter tuning and stratified cross-validation.

Computationally speaking, it is quite expensive to construct feature vectors for the entire dataset, and then run grid search on it. Since we don't all have access to supercomputers, we should find a way to make it faster.

One such technique is out-of-core learning. The idea is to fit the classifier incrementally on smaller batches of the dataset. We will stream the documents directly in mini-batches. Note that for out-of-core learning we cannot use CountVectorizer or Tfidf since it requires us to keep all the feature vectors of the training dataset. Instead we can use HashingVectorizer.

For a bit of accuracy loss, we get much better memory efficiency. 

Another thing we can do with NLP is topic modeling, so assigning category labels to unlabeled text documents. We can consider this as a clustering task. A popular technique is latent Dirichlet allocation (LDA).

The basic idea is to try and find groups of words which appear frequently together across different documents. These groups will represent our topics. LDA takes as input a bag-of-words matrix and decomposes it into a document-to-topic matrix, and a word-to-topic matrix. The downside to this algorithm is we must tell the model the number of topics beforehand.
