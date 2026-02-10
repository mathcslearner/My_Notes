# Raschka Chap 1: Learning from Data

Machine learning is all about developing algorithms which can make sense of the vast amount of data that we possess to make predictions. Examples of applications include spam filters, voice recognition software, medical applications, computer vision systems, etc.

There are three main types of ML: supervised learning, unsupervised learning, reinforcement learning.

- Supervised learning: labelled data, direct feedback, used to predict outcome
- Unsupervised learning: no labels, no feedback, used to find hidden structure in data
- Reinforcement learning: decision process, reward system, learn best series of actions

**Supervised Learning**

The idea is to provide the model with a training dataset where all the data inputs have a label. After the training, the model should be able to make predictions on new, unlabeled data inputs. An example would be a spam filter which labels emails as either spam or non-spam. There are two kind of tasks, classification and regression.

- Classification: The data labels are discrete, unordered categories. Then the goal is just to predict the category for which an unknown input belongs in.
- Regression: Here, we are given a number of predictor variables/features and a continuous response variable/target/outcome. We want to find the relationship between them. An example would be trying to use study time to predict SAT score.

**Reinforcement Learning**

The goal of reinforcement learning is to develop an agent which improves its performance based on interacting with the environment. Upon taking an action, it receives a reward signal indicating how "good" the action is. Through interacting and trial-and-error, the agent will learn how to act in a way to maximize its reward.

An example would be a chess program which interacts with the board. The agent would need to enter a state (a move) every time.

**Unsupervised Learning**

The idea is to extract meaningful information/structure from the data without already having a known outcome at hand. Here are two common examples of unsupervised learning:

- Clustering: The goal is to organize the information into meaningful subgroups/clusters based on their similarity to each other.
- Dimensionality reduction: Sometimes we have data which comes with a high number of measurements and features, which is impractical. The goal is to remove some of these features while retaining the core relationships between the objects.

**Notation and terminology**

For datasets, we will take rows to be training examples and columns to be features. We can use a matrix notation to refer to the complete dataset. For example:

![[Pasted image 20260209213004.png]]

The subscript refers to the feature number and the superscript refers to which training example it is.

Here is some common terminology:

- Training example: An observation, a data sample
- Training: Model fitting
- Feature: Predictor variable
- Target: Outcome, output
- Loss/cost function: Error between prediction and real outcome

**ML Workflow**

![[Pasted image 20260209213258.png]]

- Preprocessing: This is one of the most important parts of any ML application. First, we need to handle missing data / duplicates. Then, we need to select the relevant features we want to examine. The data also needs to be normalized so it's on the same scale. We also need to split the data into training and test dataset.
- Training and selecting a model: It is important to try several models to check which one has the best performance, because they all have their own biases. To measure performance, we use prediction accuracy. This step also involves hyperparameter optimization to finetune the performance.
- Evaluating models: If the model performs well enough on the test set, then it can be used in the future to predict new data.
