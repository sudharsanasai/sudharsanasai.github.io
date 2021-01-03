---
title: "Why, How And When To apply Feature Selection"
categories:
  - Layout
tags:
  - content
  - image
  - layout
last_modified_at: 2018-01-31T14:28:50-05:00
---

Modern day datasets are very rich in information with data collected from millions of IoT devices and sensors. This makes the data high dimensional and it is quite common to see datasets with hundreds of features and is not unusual to see it go to tens of thousands.

![center-aligned-image]({{ '/images/feature-selection/feature-selection-iot.jpeg' | absolute_url }}){: .align-center}

Feature Selection is a very critical component in a Data Scientist’s workflow. When presented data with very high dimensionality, models usually choke because

- **Training time** increases exponentially with number of features.
- Models have increasing risk of **overfitting** with increasing number of features.

Feature Selection methods helps with these problems by reducing the dimensions without much loss of the total information. It also helps to make sense of the features and its importance.

In this article, I discuss following feature selection techniques and their traits.

1. Filter Methods
2. Wrapper Methods
3. Embedded Methods.

## Filter Methods

Filter Methods considers the relationship between features and the target variable to compute the importance of features.

### F Test

F Test is a statistical test used to compare between models and check if the difference is significant between the model.

F-Test does a hypothesis testing model **X** and **Y** where **X** is a model created by just a constant and **Y** is the model created by a constant and a feature.

The least square errors in both the models are compared and checks if the difference in errors between model **X** and **Y**are significant or introduced by chance.

F-Test is useful in feature selection as we get to know the significance of each feature in improving the model.

Scikit learn provides the **Selecting K best** features using F-Test.

`sklearn.feature_selection.f_regression`

For Classification tasks

`sklearn.feature_selection.f_classif`

There are some drawbacks of using F-Test to select your features. F-Test checks for and only captures linear relationships between features and labels. A highly correlated feature is given higher score and less correlated features are given lower score.

1. Correlation is highly deceptive as it doesn’t capture strong non-linear relationships.
2. Using summary statistics like correlation may be a bad idea, as illustrated by [Anscombe’s quartet](https://en.wikipedia.org/wiki/Anscombe%27s_quartet).

![center-aligned-image]({{ '/images/feature-selection/feature-selection-anscombe-quartlet.png' | absolute_url }}){: .align-center}

{:.image-caption}
*Francis Anscombe illustrates how four distinct datasets have same mean, variance and correlation to emphasize ‘summary statistics’ does not completely describe the datasets and can be quite deceptive.*

### Mutual Information

Mutual Information between two variables measures the dependence of one variable to another. If **X** and **Y** are two variables, and

1. If **X** and **Y** are independent, then no information about **Y** can be obtained by knowing **X** or vice versa. Hence their mutual information is **0**.
2. If **X** is a deterministic function of **Y**, then we can determine **X** from **Y** and **Y** from **X** with mutual information **1**.
3. When we have Y = f(X,Z,M,N), 0 < mutual information < 1

We can select our features from feature space by ranking their mutual information with the target variable.

Advantage of using mutual information over F-Test is, it does well with the non-linear relationship between feature and target variable.

Sklearn offers feature selection with Mutual Information for regression and classification tasks.

`sklearn.feature_selection.mututal_info_regression 
 sklearn.feature_selection.mututal_info_classif`

![center-aligned-image]({{ '/images/feature-selection/feature-selection-mutual-info.png' | absolute_url }}){: .align-center}

{:.image-caption}
*F-Test captures the linear relationship well. Mutual Information captures any kind of relationship between two variables. [http://scikit-learn.org/stable/auto_examples/feature_selection/plot_f_test_vs_mi.html](http://scikit-learn.org/stable/auto_examples/feature_selection/plot_f_test_vs_mi.html)*


### Variance Threshold

This method removes features with variation below a certain cutoff.

The idea is when a feature doesn’t vary much within itself, it generally has very little predictive power.

`sklearn.feature_selection.VarianceThreshold`

Variance Threshold doesn’t consider the relationship of features with the target variable.

-----

## Wrapper Methods

Wrapper Methods generate models with a subsets of feature and gauge their model performances.

### Forward Search

This method allows you to search for the best feature w.r.t model performance and add them to your feature subset one after the other.

![center-aligned-image]({{ '/images/feature-selection/feature-selection-forward-search.png' | absolute_url }}){: .align-center}

{:.image-caption}
*Forward Selection method when used to select the best 3 features out of 5 features, Feature 3, 2 and 5 as the best subset.*

For data with n features,

->On first round ‘n’ models are created with individual feature and the best predictive feature is selected.

->On second round, ‘n-1’ models are created with each feature and the previously selected feature.

->This is repeated till a best subset of ‘m’ features are selected.

### Recursive Feature Elimination

As the name suggests, this method eliminates worst performing features on a particular model one after the other until the best subset of features are known.

![center-aligned-image]({{ '/images/feature-selection/feature-selection-recursive-elimination.png' | absolute_url }}){: .align-center}

{:.image-caption}
*Recursive elimination eliminates the least explaining features one after the other. Feature 2,3 and 5 are the best subset of features arrived by Recursive elimination.*

For data with n features,

->On first round ‘n-1’ models are created with combination of all features except one. The least performing feature is removed

-> On second round ‘n-2’ models are created by removing another feature.

Wrapper Methods promises you a best set of features with a extensive greedy search.

But the main drawbacks of wrapper methods is the sheer amount of models that needs to be trained. It is computationally very expensive and is infeasible with large number of features.

--- 

## Embedded Methods

Feature selection can also be acheived by the insights provided by some Machine Learning models.

**LASSO Linear Regression** can be used for feature selections. Lasso Regression is performed by adding an extra term to the cost function of Linear Regression. This apart from preventing overfitting also reduces the coefficients of less important features to zero.

![center-aligned-image]({{ '/images/feature-selection/feature-selection-regularization.png' | absolute_url }}){: .align-center}

{:.image-caption}
*The highlighted term has been added additionally to Cost Function of Linear Regression for the purpose of Regularization.*

![center-aligned-image]({{ '/images/feature-selection/feature-selection-shrinkage.png' | absolute_url }}){: .align-center}

{:.image-caption}
*As we vary ƛ in the cost function, the coefficients have been plotted in this graph. We observe that for ƛ ~=0, the coefficients of most of the features side towards zero. In the above graph, we can see that only ‘lcavol’, ’svi’, and ‘lweight’ are the features with non zero coefficients when ƛ = 0.4.*

**Tree based models** calculates feature importance for they need to keep the best performing features as close to the root of the tree. Constructing a decision tree involves calculating the best predictive feature.

![center-aligned-image]({{ '/images/feature-selection/feature-selection-tree-based.png' | absolute_url }}){: .align-center}

{:.image-caption}
*Decision Trees keep the most important features near the root. In this decision tree, we find that Number of legs is the most important feature, followed by if it hides under the bed and it is delicious and so on. [https://www.safaribooksonline.com/library/view/data-science-from/9781491901410/ch17.html](https://www.safaribooksonline.com/library/view/data-science-from/9781491901410/ch17.html)*

The feature importance in tree based models are calculated based on **Gini Index**, **Entropy** or **Chi-Square** value.

--- 

Feature Selection as most things in Data Science is highly context and data dependent and there is no one stop solution for Feature Selection. The best way to go forward is to understand the mechanism of each methods and use when required.
I mainly use feature selection techinques to get insights about the features and their relative importance with the target variable. Please comment below on which feature selection technique do you use.
