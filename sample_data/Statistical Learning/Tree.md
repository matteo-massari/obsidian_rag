# Tree based methods 
Tree can be used for regression and classification
The main idea is to divide the feature space into (simple) region. The predicted outcome y or class depends on the region on observation x falls into:
The partitioning is the result of sequential splitting rule 
![[ChatGPT Image Apr 23 2025 from Ridisegnare grafico alberi.png|300]]
## Decision tree model
Example: Predicting next year salary of baseball players based on this year's statistics.

**Predictors:**
- Predictor 1: Number of years played in Major League
- Predictor 2: Number of Hits
- Response: Next year salary (in thousands)
**Output**
If there are 3 terminal node, is equivalent to have a partition in 3
![[Screenshot 2025-04-23 at 11.05.22.png]]

### Fitting a tree
Fitting a tree to data, consist in two steps
1) Dividing the space into 3 non-overlapping region
2) Predict a value

Use a tree for prediction, given a tree with J region $R_{1},...,R_{j}$, then:
#### Regression
Given a new observation $\underline{x}$, consider the region it falls into, and predict y with the average response of all the training observation 

#### Classification 
Given a new observation $\underline{x}$, consider the region $R_{j}$ it falls into and make a prediction based on: $P(C|R_{j})$ = proportion of the training observation in the region $R_{j}$ that belong to class c for c = 1,...,k

### Build partitioning
The aim is to divide space into box
Two key aspect 
1. [[#Choose the optimality criterion]] by looking to all possible partitioning criterion
2. [[#Divide a computational efficient procedure]] 

#### Choose the optimality criterion
##### Regression: 
The natural choice under a squared error loss is to find regions $R_{1},...,R_{j}$ that minimize the $$RSS = \sum_{j = 1}^{j}\sum_{i \in R_{j}}^{}(y_{i}- \hat{y}_{R_{j}})^{2}$$ with $\hat{y_{R_{j}}}$ the main of the response in region $R_{j}$

##### Classification:
The natural choice is the **Misclassification Error**: $$E = \sum_{j = 1}^{j}E_{j} \text{ where }E_{j} = 1-\max_{c}\hat{p}(c|R_{j}), \ R_{j} = 1,...,j$$
- E: proportion of miss classification
- $E_{j}$: small if one of the predictor is small, "pure" region will have $E_{j}$ close to zero
This function is not smooth, so not very goof for optimization purpose due to the corner:
	Example, two classe: $\hat{p}(1|R_{j}), 1-\hat{p}(1|R_{j})$ $E = 1- \max\{\hat{p}(1|R_{j}), 1-\hat{p}(1|R_{j})\}$ ![[Screenshot 2025-04-23 at 12.00.03.png|300]]

two smooth alternatives are:
1) **Gini index** $$G = \sum_{j=1}^{j}G_{j} \text{ with } G_{j}= \sum_{c = 1}^{k}\hat{p}(c|R_{j})(1-\hat{p}(c|R_{j}))$$![[Screenshot 2025-04-23 at 13.37.12.png|300]]
2) **Entropy** $$D = \sum_{j = 1}^{j}D_{j} \text{ with } D_{j} \sum_{c = 1}^{k} \hat{p}(c|R_{j})\log(\hat{p}(c|R_{j}))$$

 ![[Screenshot 2025-04-23 at 13.37.33.png]]

#### Divide a computational efficient procedure
The next step is to find the partitioning that minimises the chosen optimality criterion.  This is a search among a huge number of partitions, which is not feasible.
The main algorithm is **Recursive binary splitting**
- top-down approach (start with everything in a region)  
- binary → at each step one more region is created  
- greedy (local)

**Algorithm** -> Recursive binary splitting
1) For each predictor $x_{k}$ and all its cut points define the two regions: $$R_{-}(k,s) = \{x:x_{k}<s\} \text{ and }R_{+}(k,s) = \{x:x_{k}>s\}$$ In practice:
	- For continuous variable a discrete sequence of cut off
	- for categorical variable it all spilt the category in two groups
	We find the optimal split point $s$ by computing an optimality criterion for each $(k, s)$ pair, and selecting the partition that minimizes the loss. So minimize this formulation:
		$$min_{j,s} = \left[min\sum_{i:x_{i}\in R_{1}(j,s)}^{}(y_{i}- \widehat{y}_{R_{1}})^{2}+ min\sum_{i:x_{i}\in R_{2}(j,s)}^{}(y_{i}- \widehat{y}_{R_{2}})^{2}\right]$$
2) Assign to each region an $\hat{y}$ as the most present value y in a region. 
3) Repeat the procedure in each region and choose best partitions
4) Continue until a stopping criterion is satisfied:
	- maximal number of observation in each regions 
	- optimality criterion not sufficiently reduced

Computational trick
Save the result to save computations 
![[Screenshot 2025-04-23 at 14.38.27.png]]

#### Issues with trees
The model have a to high variance so it tend to overfit the data:
Some solution:
1) ==Fit a smaller size of the tree==
2) Stop dividing when reduction in ==gini \ entropy== exceeds some high threshold the problem is that an "unimportant" split early could result in a good split later
3) Build a big tree and prune it back -> ==Cost complexity pruning==
	1. **Fix a hyperparameter** α ≥ 0
		 - If $\alpha = 0⇒ T_{\alpha} = T_{0}$ 
		- As $\alpha$ increases, $T_\alpha$ becomes smaller (pruning happens in nested fashion)
		- Should be ==chosen via cross-validation== to find the best bias-variance trade-off.
	2. **Algorithm:**
	   - Use **recursive binary splitting** to grow a very large tree. Denote this tree as **T<sub>0</sub>**
	   - Choose the **subtree** $T_{\alpha} ⊆ T_{0}$ such that: $$ T_α = \arg\min_{T \subseteq T_0} \left( \sum_{j=1}^{|T|} Q(R_j) + \alpha |T| \right) $$where:
	     - $Q(R_{j})$: impurity or error in region j
	     - $α · |T|$: penalization for model complexity  *(this is the cost-complexity optimality criterion)*


How to choose $\alpha$
We choose a **sequence of α values**, and for each α:
1. Fit the tree on **k-1 folds** and prune it → use $T_\alpha$ to make predictions on the **fold left out**
2. **Calculate CV error** for each α

==other info==
- *misclassification error* rate corresponds to the fraction of times an observation ends up in the wrong region and it is thus misclassified
- *residual mean deviance* is the total entropy divided by the number of observations minus the number of terminal nodes (degrees of freedom)

## Bagging (Bootstrap Aggregation)
Bagging is a technique that reduce the variance of statistical learning method, without increasing bias. The only disadvantage is in a loss of interpretability

Motivation: **Variance and bias trade-off**
Assume that we have n independent random variable that produce  $z_{1},..,z_{n}$ estimation all with the same variance $\sigma^{2}$ that produce a big estimation $Z$
Then
$$Var(\overline{Z}) = var\left(\frac{\sum_{i = 1}^{n}z_{i}}{n}\right)= \frac{n\sigma^{2}}{n^{2}} =\frac{\sigma^{2}}{n} $$ So Variance of the sample mean is smaller by a factor 1/n 
$$
E(\overline{Z}) = E\left(\frac{\sum_{i = 1}^{n}Z_{i}}{n}\right) = \frac{n\mu}{n} = µ 
$$
For the mean there is no increase in bias 

In our context ...
Consider B an independent training sets, we could then build B model, one for each dataset $\hat{f}_{1},...,\hat{f}_{B}$ and average them to reduce the variance. 
$$
\hat{f}_{avg}(\underline{x}) = \frac{1}{B} \sum_{i = 1}^{B} \hat{f}_{i}(\underline{x})
$$
Taking the average i am reducing the variance but i don't have an independent training set. So i can use bootstrap,

### Bootstrap
Is a resampling methods that involve in build B training set of the same size as the original dataset by repeatedly sampling with replacement from the dataset that we have. 

**Iterative process**
1) I have n rows of a dataset. 
2) the computer randomly pick a row from 1 to n 
3) Now i sample one row in a new dataset e put it back
4) iterate the procedure for n times 
5) i will get a new dataset with random sampling
![[Bootstrapping.png|300]]
Obviously the new dataset is not the same because, some rows are not picked, other could be picked twice ecc ecc. But at least i am sure that the come from the same source, so the stochastic process behind them is preserved 

Build a model for each bootstrapped dataset, $\hat{f}_{i}^{*}$, and average the prediction.
$$
\hat{f}_{bag}(\underline{x}) = \frac{1}{B}\sum_{i = 1}^{B}\hat{f}_{i}^{*}(\underline{x})
$$
Some Remarks
1) For classification, $\hat{f}_{i}^{*}(\underline{x})$ can be taken either as the estimated probability $\hat{p}_{i}^{*}(c|\underline{x})$ for each class c or the predicted class for each tree (after selecting a threshold). In the second case, you can take the majority vote as the predicted class
2) The B bootstrapped dataset will not be independent, so we do not get a full reduction in variance of $\frac{1}{B}$ so if there are too correlated the variance will not decrease as much.
3) Bagging reduce variance without increasing bias
4) B should be taken as large computationally possible (no issue with overfitting)

## Out Of Bag
Having built a model by bagging, we want to test its prediction accuracy on unseen data. We can do cross-validation, but it is computationally expensive. An efficient alternative is to make use of boosting resampling 

Note of boostrap:
Each bootstrapped dataset han n observation. Consider $x_{i}$ observation form the original dataset. What is the possibility that it does not appear in a given bootstrapped sample.
$$
\left(\frac{n-1}{n}\right)^{n} \longrightarrow_{n \to \infty} e^{-1} \approx 0.363
$$
that is quite large, so the conclusion is that each sample will use approximately 63% of the data. thats mean that the model that i will fit thought this data won't see n the 37% of the observation, so i can test model with unseen data. 

We call the unused observation out-of-bag observation and we can evaluate the model on these observation 

For each observation $\underline{x}$, make a prediction based on the trees that save been fitted on sample that are not included on $\underline{x}$. Calculate error ROC ext based on the prediction.

Cost of bagging (disadvantage)
1) Loss of interpretability: average of a tree is not a tree
	however we can calculate a varous measure of variable importance 
	for each predictor 
	Add up the total amount that the RSS / Gini / Entropy ... is decreased by split on that given predictor arranged across B trees
2) Bootstrap samples are correlated and not independent

note: 
$z_{1},...,z_{n}$ are now correlated, For simplicity, assume $Cor(z_{i},z_{j}) = \delta> 0$ 
$$
\begin{align}Var \sum_{}^{}z_{i} \text{ due to there are no more independent} 
\\= \sum_{i,j}^{}cov(Z_{i},Z_{j}) = \sum_{i = 1}^{n}Var(Z_{i}) + 2cov(Z_{i},Z_{j}) \\
= n\sigma^{2}
\end{align}$$
From the correlation e can perform the covariance =
$$
Cor(z_{i},z_{j}) = \frac{cov(z_{i},z_{j})}{\sqrt[]{var(z_{i})var(zj)}} = \frac{cov(z_{i},z_{j})}{\sigma^{2}}
$$
That in pur formula would be 
$$
n\sigma^{2} + 2 \sum_{i<j}^{}\delta \sigma^{2} = n \sigma^{2} + n(n-1)\delta \sigma^{2}
$$
If we take the sample mean
$$
var \frac{\sum_{i = 1}^{n}z_{i}}{n} = \frac{\sigma^{2}}{n} + \frac{\cancel{n}(n-1)}{n^{\cancel{2}}}\delta \sigma^{2} = \frac{\sigma^{2}}{n} + \frac{(n-1)}{n}\delta \sigma^{2}
$$
by increasing n $\frac{\sigma^{2}}{n}$ goes to 0 and $\frac{(n-1)}{n}\delta \sigma^{2}$ goes to $\delta \sigma^{2}$ so the variance cannot be reduced effectively.

## Random forest
random forest provide a way to decorrelate trees. 
Why would the trees grown in bagging be correlated? Strong predictors will find to appear at the initial split oof the trees w

Idea Before each split in each tree, ==draw randomly m features== at random from the p predicts and look for split among these variables only

Remark: f there is a strong predictor it will now appear at the first split only on $\frac{m}{p}$ trees out of the B that are growth. m has now a role on bias variance trade off

Choice of m 
1) m = p is bagging, so random forest is generalization of bagging
2) typically default values:
	- Classification : $m= \sqrt[]{p}$
	- Regression: $m = max\{\frac{p}{3},1\}$
3) The best strategy is to find cross validation so that we find the optimal trade-off on any given dataset. Indeed, m is also related to the number of strong predictors in the dataset:
	- Many, then m should be set to a small value 
	- Few, then m should be highest

# Boosting 
(addestrare questi alberi)
Boosting is beyond the idea of aggregating trees that are sequentially grown from the same data. Each tree id fitted on the residual of the pervious one. 
Idea use weak learners (simple models) and update these slowly by fitting a tree residuals of the previous model. 

## Gradient boosting algorithm (regression trees)

Given the loss $L(y_{i}, \hat{y}_{i}) =(y_{i} - \hat{y}_{i})^{2}$ :
1) ==Initialize $f_{0}(\underline{x}_{i}) = \arg \underset{\gamma}{\min} \sum_{i = 1}^{n}(y_{i}-\gamma)^{2}$ as a constant model==. So we initialize $f_{0}$ as the sample mean of $\overline{y}$. In an alternative formulation we can initialize the model as $f_{0}(\underline{x}_{i}) = 0$
2) ==With an iterative update, i upgrade my model to best perform my $\hat{y}$==  as: For $m = 1,..,M$. m is an hyperparameter that measure out the number of iteration, so the number of weak learners that have to be summed. 
	1) ==Calculate the residual== $rim = y_{i} - f_{m-1}(\underline{x}_{i}), \ \ i=1,...,n \ \ \text{residuals}$ 
		N.B. The real algorithm uses the pseudo-residuals  $r_{i} = -\frac{1}{2} \frac{\partial \mathcal{L}(y_{i}, \hat{f}(\underline{x}_{i}))}{\partial \hat{f}(\underline{x}_{i})}$ 
	2) ==Fit a residual regression tree== $h(x)$(weak learner) to the residuals (training data $(x_{i},r_{i})$), giving terminal regions $Rjm, \ \ j = 1,2,...,j_{m}$. 
		- $J_{m}$ is an hyperparameter, it measure the interaction depth so it tune the complexity of the model 
		- At this stage, each leaf $R_{jm}$of the residual regression tree contains a group of training examples $\mathbf{x}_i$, each with an associated true target value $y_i$​. 
	3) ==Then for each region find on h(x) $j = 1,...,R_{jm}$, we compute the correction value== as $\gamma_{jm} = \arg\min_{\gamma} \sum_{\underline{x}_i \in R_{jm}} \mathcal{L}(y_{i}, f_{m-1}(\underline{x}_{i}) + \gamma)$ where:
		- Since all the examples in a given leaf will receive the **same correction** $\gamma_{jm}$​, we aim to find the value of $\gamma$ that, when added to the current model prediction $f_{m-1}(\mathbf{x}_i)$, best reduces the loss function for **all the observations in that leaf**. This means computing the value $\gamma_{jm}$​ that minimizes the total error between the corrected predictions and the true targets within that region.
		- So we choose $\gamma_{jm}$ by find a value $\gamma$ (so the leaf that minimize the error) by computing the loss, comparing the real value of the dataset
		- Also $\sum_{\underline{x}_i \in R_{jm}}$ mean that we are picking the sample associated to that leaf
	4) ==We assign a correction value to the tree output as==  $h(x) = \sum_{j=1}^{m} \gamma_{jm} \cdot I(\underline{x}_{i} \in R_{jm})$ where is: 
		- filtered by $I(\underline{x}_{i} \in R_{jm})$, that allow to assign $\gamma_{jm}$ only to the interested observation. 
		$$h_m(x; \theta_{m}) =\begin{cases} \gamma_{1m} & \text{if } x \in R_{1m} \\ \gamma_{2m} & \text{if } x \in R_{2m} \\ \gamma_{3m} & \text{if } x \in R_{3m} \end{cases}$$
	5) ==Update the classification tree== as $f_{m}(\underline{x}_{i}) = f_{m-1}(\underline{x}_{i}) + h(x)$
		We make a new prediction for each sample based on the last prediction $f_{m-1}(\underline{x}_{i})$ 
3) ==Output the model== $\hat{f} (\underline{x}_{i})= \hat{f}_{m}(\underline{x}_{i})$

![[Screenshot 2025-07-06 at 10.33.40.png]]

Remarks
1) The loss can take different forms. In classifications, the exponential loss is used in popular AdaBoost. $e^{-y_{i}f()}$ 
2) A common version of boosting has an additional parameter, called learning rate, so instead d it uses the (d*) $f_{m}(\underline{x}_{i}) = f_{m-1}(\underline{x}_{i}) + v\sum_{j = 1}^{m}\gamma_{jm}I(\underline{x}_{i} + R_{jm}) \ \ 0<v<1$ Usually v is get to a small value and M is then tuned
3) Stochastic gradient boosting, where step (b) consist of fitting a tree only to a random sample of data. This add stochasticity as well as providing out of sample data to evaluate wheter to proceed to the most iteration

**Hyper parameter summary**
- m = tune the number of iteration, so of weak learners that compose the final model. Is related to bias/variance trade-off:
	- M too small: the model is simple, so we have an high bias and it eill underfit
	- M is too big: the model is getting more and more complex adaptingo too weel to the data, with an high variance. So it will overfit
	Should be chosen by cross-validation
- J = is the interaction depth, measure out the complexity of the single weak learner. Is typically small (1,2,3)
- $\lambda$ also called the shrinkage parameter (e.g., 0.001, 0.01), controls how much each iteration contributes to the final model. It is used to reduce overfitting: by limiting the impact of each individual tree, we require more iterations to reach the same level of accuracy, which allows for finer and more stable learning.

**Evaluating two model**
To evaluate to model, (e.g., Random forest and a tree fitted with GBM) we cannot use the CV integrated on the GBM model. The reason are:
- Every CV used in the GBM is random and unique for the model. so also if we set a seed, the random forest will create different for from the GBM. 
- The Random forest does not use the CV error but will use the out-of-bag error, thath are based to observation that are not included on the bootstrap process. So we cannot compare the two values
	NOTE 
	The bootstrap compiute dataset of the same length of the original by sampling the original dataset. The probability that each observation does not appear in the dataset are $\left(\frac{n-1}{n}\right)^{n} = e_{-1}$ that are nearly the 37%. So the excluded observation are included in the test set

Indeed an option is to perform a fixed CV outside the CV proposed by the model, where the user decide the number of fold and how to create the different dataset. in this case i will have a cv and a complelty unseen test set that i can use to evaluate the model 

$$
\text{MeanDecreaseGini}(X_j) = \frac{1}{\text{\#splits on } X_j} \sum_{\text{tree, split}} \Delta \text{Gini}
$$