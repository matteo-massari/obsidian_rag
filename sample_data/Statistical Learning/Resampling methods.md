# Evaluating models
Different model are associated to a different level of complexity (number of parameters, k) and this achieve a certain bias variance trade off.

Visualisation:

| K-NN                                                                                                                        | Polynomial Regression                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| ![[Pasted image 20250328163004.png\|300]]                                                                                   | ![[Pasted image 20250328163014.png\|300]]                                                                       |
| - Blue line: small k, large number of parameter (low bias, high variance)<br>- Red line: large k, small number of parameter | - Blue line: seems to be a good approximation<br>- Green line: high degree so overfit<br>- Red line: degree = 1 |
The general idea is to measure the accuracy of the model in unseen data (test data)
![[Test_and_train_error.png|300]]

We want to achieve the minimum error level, via a sort of complexity, gained by the tuning of the hyperparameter via test data.

> [!warning]
> We use the test data to train the hyperparameter, and it does't make sense train it with the train data. 
> 

The best solutions in to have a large disengaged test set, left aside to select the optimal model. Often this is not available 
The two main approaches:
1) make some adjustments to the training error in order to estimate the test error rate. Some measure are Aic, Bic that adjust automatically the paramenter of the model. Is for large n
2) [[Resampling methods]]: estimate directly the test error rate by revising some kind of Resampling strategies. 

# Resampling methods
General idea: the subset of the data from the fitting process, so that we can use these observation to test the fitted model 
There are various version of this idea, that can reach a compromise between 
1) Computational complexity 
2) Using enough data for fitting and testing 
3) Account for variation due to a specific sampling 

## Different strategies
### Validation set approach 
Shuffle the entire dataset and randomly dived into 2 part: 
- Training dataset
- Test dataset 
The proportion can be 50% 50%, or different like 70% 30%
So the model is fitted on training data and tested on validation set

**Example**
Polynomial regression:
- Fix d
- Train $\underline{\hat{\beta}}$ on the training data
- Confront $\hat{y} = \underline{x}^{t}\underline{\hat{\beta}}$ with the y and the x on the validation set
- compare the $\hat{y}$ with the y -> MSE
- Iterate for a number of d and choose the model with the least MSE

K-nearest neighborhood 
- Fix a number of k
- Calculate $p(1|\underline{x})$ for $\underline{x}$ in a validation set, using k ttrining point close to x
- Calculate the error test
- Choose the optimal k

**Problems**
![[Pasted image 20250328165622.png]]
We should be aware of the variability caused by the random shuffling of the data. To solve this problem we can iter the various seeds and repeat the train-validation split many times and draw some robust conclusion from all the curves

### K-fold cross validation
The k-fold cross validation is a sophisticated way to solve this problem. 

1) Randomly divide the data part in equally side 
![[K-fold example.jpg|300]]
2) Fit the model on the remaining K-1 parts 
3) Use the model to make prediction on the last part of the dataset and calculate the MSE
	Let $C_{1},...C_{k}$ be the indexes of the observations for each folds 
	For each folds $C_{i}$ calculate the error
$$
\begin{align}
&E_i = 
\begin{cases}
\text{MSE}_i = \frac{1}{|C_i|} \sum_{j \in C_i} (y_j - \hat{y}_j)^2 \\
\text{ER}_i = \frac{1}{|C_i|} \sum_{j \in C_i} \mathbb{I}(y_j \ne \hat{y}_j)
\end{cases} \\
&\text{Where }\hat{y}_{j} \text{ is the prediction from the model fitted on all folds except i}  = 1, \ldots, K
\end{align}
$$
4) Repeat across all folds 
5) Combine the results
$$
CV = \frac{1}{k}\sum_{i = 1,..,k}^{k}E_{i}
$$

My expectation is to find a U shape curve:
![[U-shape cross validation.jpeg|300]]

Why use the K-fold?
- The fitting sees the entire dataset
- Low variability of CV error
- Can be more computationally expansive on a single validation split 

The best number of fold seems to be 5 to 10

### Leave-one-out cross validation
In the Leave-one-out cross validation, the number of k = n, so there are no random sampling. Each observation belongs to one fold. It's computationally expensive, except for linear regression where there is a formula:
$$
LOOCV = \frac{1}{n} \sum_{}^{} \left(\frac{y_{i}- \hat{y_{i}}}{1-b_{i,j}}\right)^{2}
$$
where:
- $y_{i}- \hat{y_{i}}$ predicting using $\hat{\beta}$ fitted on the full data
- $b_{i,j}$ Leverage

Higher variability due to the sum of cumulated errors

### Nested cross validation
Is the Cross validation for model comparison
**Example** 
We perform a cross-validation to obtain an estimate of the error rate on unseen data
![[Resampling methods.jpeg|300]]

This comparison could be seen as unfair towards logistic regression, since the cross validation procedure to select k has seen the full data

A most advanced approach is to use the nested cross validation, 

In its simplest form the test are randomly splitted:
- Training set
- Validation set
- Test set

The test dataset is left behind to perform a final comparison, including tuning of the hyperparameters. So for the K-nn:
1) Select k on training - validation split
2) Evaluate optimal model on test data

This can be done in a more advanced way:
1) Take a fold out
2) Perform k-fold cross validation on remaining k-1 folds to select the tuning parameter
3) Use the optimal model to predict fold left out 
4) Iterate across all fold

## Bootstrap
