# How to Deal with imbalanced classes
## Motivation
Training data sets for classification models will rarely come with equal classes. What problems will occur if you use data that is not balanced? What happens the more extreme the imbalance gets?

If you build a fraud classification model, as in our most recent case study, and the training data is heavy in not-fraud data, will the resulting model miss (expensive) cases of fraud?

## Common Examples   

* Medical screening
* Customer churn prediction
* Online ad conversion
* Credit card fraud detection

_When is a class considered imbalanced?  Research usually draws the line at a minority class < 10-20%._

## The Accuracy Paradox
In any one of the above examples, the minority class could easily be 5% (or less) of the total dataset.  For a sample with this 95/5 split, a model that always predicts the majority class would have a 95% accuracy!

It would be right 100% of the time for the majority class, but 0% of the time for the minority class.

## Methods
Our research didn't turn up a singular, definitive solution to this problem which suggests that it changes case by case. There was a consensus on available/proven methods:

- Compare classification models with confusion matrix and appropriate metrics
- Tune your models threshold by creating an ROC curve
- Undersample and/or oversample your training data to create a more balanced dataset
- Try a tree-based model - their heirarchical structure often allows them to pick up on signals from both classes
- **Compare classification models with cost/benefit matrix**

## Confusion Matrix and metrics

![Confusion Matrix for binary classification](confusion_matrix.png)

Choosing appropriate metric depends on the problem. For fraud classification where the fraud is very expensive, minimize the miss rate (FN/FN+TP). See [wikipedia confusion matrix page](https://en.wikipedia.org/wiki/Confusion_matrix) for full list.

## ROC Curve

![ROC Curve example](roc.png)

To construct ROC Curve:
1. Train a classification model and get the numerical values (probabilities) for each data point classification
2. Sort numerical values so lowest is first
3. Build plot data by iterating over i with the following steps:
  - Take i value as classification threshold
  - Everything before i is class 0, everything after is class 1
  - Calculate new TPR, FPR, save to plot data

[See Listing 7.5 in Machine Learning Book for details](https://drive.google.com/file/d/0B1cm3fV8cnJwcUNWWnFaRWgwTDA/view)

## Balance the classes

- **Undersample** the majority class by deleting examples. Could delete part of signal
- **Oversample** the minority class by duplicating that class data or using it to add new minority data  

An important oversampling technique is SMOTE, or Synthetic Minority Oversampling Technique.  Uses kNN to create 'synthetic' data points using the real minority class data.
![SMOTE](https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/03/16142852/ICP3.png)

There is an ['UnbalancedDataset'](http://contrib.scikit-learn.org/imbalanced-learn/stable/) module available for Python, with implementations of SMOTE and other resampling techniques.

## Cost/Benefit Matrix

- Just a weighted confusion matrix
- Be careful when constructing a cost/benefit matrix: don't double count a value
- Include in classification algorithm:
  - Adaboost has error weight vector, D
  - Naive Bayes could predict on lowest cost rather than highest probability
  
## Conclusion

- Understand your data, your model, and your model evaluation metrics
- Since there's no one-size-fits-all solution, try several different balancing techniques and models and see what work best!
