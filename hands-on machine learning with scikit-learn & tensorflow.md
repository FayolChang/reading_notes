# hands-on machine learning with scikit-learn & tensorflow

# part I Fundamental of Machine learning

# chap1 the machine learning landscape

本章主要给出了机器学习的定义，以及为什么要用机器学习。

机器学习的类型：监督/非监督/半监督/增强学习；batch=offline/online; instance-based/model-based;

机器学习的挑战：

- ​	训练数据不足

- ​	训练数据没有代表性

  ​		数据质量很差

  ​		不相关的特征

- ​	过拟合

- ​	欠拟合

测试与验证

# chap 2 end-to-end machine learning project 

本章重点介绍了机器学习的流程。从界定问题开始，到评价标准，到搜集数据等等

Working with the real data

Look at the Big Picture

​	Frame the problem

​	Select a Performance Measure

​	Check the assumption

Get the Data

​	create the workspace

​	download the data

​	Take a quick look at the data structure

​	Create a Test set

Discover and visualize the Data to Gain Insights

​	visualizing Geographical data

​	looking for correlations

​	Experimenting with Attribute combinations

Prepare the data for machine learning algorithms

​	Data cleaning

​	Handling Text and Categorical Attribute

​	Custom Transformers

​	Feature Scaling

​	Transformation Pipelines

Select and Train a Model

​	Training and Evaluating on the Training Set

​	Better Evaluation Using Cross-validation

Fine-Tune your model

​	Grid Search

​	Randomized Search

​	Ensemble Methods

​	Analyze the Best models and Their Errors

​	Evaluate Your System on the Test Set

Launch, Monitor, and Maintain you System

Try it Out!

# chap3 Classification

本章利用MNIST数据集，介绍了Accuracy, Confusion Matrix, Precision, Recall, ROC, Multiclass Classification.

在数据集不平衡时，Accuracy不是一个好的评判标准。

Precision Recall Trade-off : Precision (y axis) vs Recall(x axis)

ROC curve: True Positive Rate(=Recall=Sensitivity) y-axis vs False Positive Rate (=1-Specificity)

# Chap4 Training Models

主要有Gradient Descent, Stochastic Gradient Descent, Batch Gradient Descent, Mini-batch Gradient Descent.

Learning Curve: x轴样本量

L1-norm, L2-norm, Elastic Net, Early Stopping.

Decision Boundaries, Softmax regression

Cross Entropy
$$
H(p, q) = - \sum_{x}p(x)log \, q(x)
$$
Cross Entropy cost function
$$
J\theta) = - \frac{1}{m} \sum_{i=1}^{m}\sum_{k=1}^{K}y_{k}^{(i)}log\big(\hat p_{k}^{(i)}\big)
$$
Cross Entropy gradient vector for class k
$$
\nabla_{\theta_{k}} J(\theta) = \frac{1}{m}\sum_{i=1}^{m}\left(\hat p_{k}^{(i)} - y_{k}^{(i)}\right) \mathbf x^{(i)}
$$

# Chap5 Support Vector Machines

没什么说的

# Chap6 Decision Trees

Gini, Entropy

# Chap7 Ensemble Learning and Random Forests

介绍了VotingClassifier, Bagging and Pasting.

Bagging: 同一个算法，有放回抽样

Pasting: 同一个算法， 无放回抽样,Scikit-learn BaggingClassifier中，bootstrap=False即可。

Out of Bag Evaluation: 计算oob_score_

Random Patches: Sampling Both training instances and features

Radom Subspaces: keep all instances but sampling features

Random Forest: searches for the best feature among a random subset of features.

Extra-Trees: using random thresholds for each feature rather than searching for the best possible thresholds.

Boosting:

​	adaboost

​	gradient boost

Stacking: