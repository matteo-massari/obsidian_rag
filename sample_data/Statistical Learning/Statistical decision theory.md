# Statistical decision theory
We will concentrate on **supervised learning**, we are given both x and y and we use this information to learn the relationships between x and y. 
Supervised method can be divided into two types:
- ==regression methods==: y is quantitative (numerical) 
- ==Classification methods==: y is qualitative (categorical, typically binary)
no assumptions are made on x, the vector of predictors 

The objective: given $(\underline{x}_{i}, y_{i}), i = 1,...,n$ find a rule $\hat{f}$ that allows to predict y from $\underline{x}$:
$$\hat{y} = \hat{f}(\underline{x})$$
- Regression: regression function 
- Classification: classification function 

So the main point is to find a $\hat{f}$ is an approximation of some true $f$. Statistical decision theory formalize the type of relationship that $\hat{f}$ would like to learn
The measurement of the error is the [[Introduction to Machine Learning#Cost Loss function |Cost loss function]]
The **Cost \ Loss function** is used to ==measure the performance of the estimator== (the quality of the fit) ==thought the measurement of the error== 

## Regression Setting
In the regression setting the y is quantitative

To asses when a model is the best in the regression setting is used the **Mean Squared Error**:
$$
MSE = \frac{1}{n} \sum_{i = 1}^{n}(y^{(t)}_{i}-\hat{f}(\underline{x_{i}})^{(t)}))^{2} \ \text{ with } (x^{(t)}_{i}, y^{(t)}_{i})
$$


Can be analyzed under various aspect:
- [[#Cost / loss function]]
- [[#Type of regression methods]]
	- [[#Linear regression]]
	- [[#k-Nearest Neighbors (k-NN) method]]
- [[#Bias - variance trade-off]] 

### Cost / loss function
In the regression setting the measurement of the error (so the accuracy of the model) is done with the **Squared Error Loss**:
$$
L[y,f(\underline{x})] = [y - f(\underline{x})]^{2} 
$$
**Expected Prediction Error**:
$$
EPE(f) = E_{\underline{x},y} [L(y,f(\underline{x}))]
$$
The goal is to ==minimize the $EPE$ value==, that can be seen also like 
$$
\int_{}^{}(y-f(\underline{x}))^{2}g(\underline{x},y)dxdy
$$
Where:
- ($y-f(\underline{x}))^{2}$ is the squared error
- $g(\underline{x},y)$ is the join density 

**Theorem of minimization** 
$E[(y-f(\underline{x}))^{2}]$ has a minimum when $f(\underline{x}) = E[y|\underline{X}=\underline{x}]$



Note that: $y = f(\underline{x}) + \epsilon, \ \ E[\epsilon] = 0, \ \ E[\epsilon]\bot \underline{x}$,  
$E[y | X = \underline{x}] = f(x)$ so x has to be evaluated as a constant. 

*Prove of the theorem:*
$$
\begin{align}
E_{\underline{x},y} [(y-f(\underline{x}))^{2}] = & \text{(tower law})E_{\underline{x}}[E_{\underline{x},y}[(y-f(\underline{x}))^{2}|\underline{X} = \underline{x}]] \\
=& \int_{}^{}\left[\int_{}^{} (y-f(\underline{x}))^{2}g_{y|\underline{x}}(y|\underline{x})dy \right]g_{\underline{x}}(\underline{x})d\underline{x}
\end{align} 
$$
- $g_{y|\underline{x}}(y|\underline{x})dy \text{  and  } g_{\underline{x}}(\underline{x})d\underline{x}$ are densities 
Consider a fixed $\underline{x}$ the inner expectation:
$$
E[(y-f(\underline{x}))^{2}|\underline{X} = \underline{x}]
$$
So we need to find a constant $a$ that minimize:
$$
\begin{align}
&E[(y - a)^{2}] = E[y^{2}] - 2aE[y] + a^{2} \\
&\text{Derive that for a to obtain the minimum } \\
&\frac{d }{da} E[(y-a)^{2}] = 0-2E[y] + 2a = 0 \\
& a^{*} = E[y]
\end{align}
$$
The value of the function at the minimum is $E[(y-E(y))^{2}] = var(y)$
So $f(\underline{x}) = E[y | \underline{X} = \underline{x}]$ is the function that minimize $E[(y - f(\underline{x}))^{2}]$


For hypotesis, other choice of loss lead to different function:
*Example*:
$$f(\underline{x}) = median[y|\underline{X}=\underline{x}]$$
- LAD regression
- Quantile regression
- rob regression

### Assessing model accurancy

$$
\text{MSE} = \frac{1}{m} \sum_{i=1}^{m} \left( y^{(t)}_i - \hat{f}(x^{(t)}_i) \right)^2
\quad \text{with } (x^{(t)}_i, y^{(t)}_i),\ i = 1, \dots, m\ \text{test data}
$$

#### Bias - variance trade-off in regression setting
In the bias and variance trade-off we decompose the ==Mean Squared Error (MSE)==
$$
\begin{align}
\text{MSE} =&\ E[(y - \widehat{f}(\underline{x}))^2 \,|\, \underline{X} = \underline{x}] \\
=&\ E[(f(\underline{x}) + \epsilon - \widehat{f}(\underline{x}))^2 \,|\, \underline{X} = \underline{x}] \\
\text{We add } & E[\widehat{f}(\underline{x})] \text{ and } - E[\widehat{f}(\underline{x})] \\
=&\ E[(f(\underline{x}) + \epsilon - \widehat{f}(\underline{x}) + E[\widehat{f}(\underline{x})] - E[\widehat{f}(\underline{x})])^2 \,|\, \underline{X} = \underline{x}] \\
\text{Then match } & \text{ the couple as:} \\
=&\ E[(\underbrace{f(\underline{x}) - E[\widehat{f}(\underline{x})]}_{a} + \underbrace{\epsilon}_{b} + \underbrace{E[\widehat{f}(\underline{x})] - \widehat{f}(\underline{x})}_{c})^2 \,|\, \underline{X} = \underline{x}] \\
\text{We apply } & (a + b + c)^2 = a^2 + b^2 + c^2 + 2ab + 2ac + 2bc \\
\text{Expanded form:} \\
=&\ E[a^2] + E[b^2] + E[c^2] + 2E[ab] + 2E[ac] + 2E[bc] \\
\text{Now:} \\
a^2 =&\ E[(f(\underline{x}) - E[\widehat{f}(\underline{x})])^2] = [Bias(\widehat{f}(\underline{x}))]^2 \\
b^2 =&\ E[\epsilon^2] = Var(\epsilon) \quad \text{(irreducible noise)} \\
c^2 =&\ E[(\widehat{f}(\underline{x}) - E[\widehat{f}(\underline{x})])^2] = Var(\widehat{f}(\underline{x})) \\
\text{Cross terms:} \\
2ab =&\ 2E[(f(\underline{x}) - E[\widehat{f}(\underline{x})]) \cdot \epsilon] = 0 \quad E[\epsilon] \text{ is 0 so it cancel the whole term} \\
2bc =&\ 2E[\epsilon \cdot (E[\widehat{f}(\underline{x})] - \widehat{f}(\underline{x}))] = 0 \quad E[\epsilon] \text{ is 0 so it cancel the whole term} \\
2ac =&\ 2E[(f(\underline{x}) - E[\widehat{f}(\underline{x})]) \cdot (E[\widehat{f}(\underline{x})] - \widehat{f}(\underline{x}))] = 0 \\
&\ \text{using the properties } E[E[Z] - Z] = E[Z] - E[Z]  = 0 \\ 
=&\  2 \cdot E[f(\underline{x})-E[\widehat{f}(\underline{x})] \cdot E[E[\widehat{f}(\underline{x})] - \widehat{f}(\underline{x})] \\
=&\ 2 \cdot E[f(\underline{x})-E[\widehat{f}(\underline{x})]] \cdot \underbrace{(E[\widehat{f}(\underline{x})] - E[\widehat{f}(\underline{x})]])}_{\text{=0}}= 0
\\
\text{So final decomposition:} \\
\text{MSE} =  &\ (f(\underline{x}) - E[\widehat{f}(\underline{x})])^{2} +Var(\widehat{f}(\underline{x})) + Var(\epsilon)  \\
&\ [Bias(\widehat{f}(\underline{x}))]^2 + Var(\widehat{f}(\underline{x})) + Var(\epsilon)
\end{align}
$$
The explanation is: 
- $[Bias(\widehat{f}(x_{0}))]^{2}$ this is the bias 
- $Var(\epsilon)$ irridiciblle error
- $var(\hat{f}(x_{0})))$ = Variance

The model complexity is explained in:
- Simple models:
	- high bias
	- low variance 
- Complex models
	- low bias 
	- high variance 



### Type of regression  methods
**Under a Squared Error Loss**, different regression methods may different assumptions about $f(\underline{x}) = E[y|\underline{X} = \underline{x}]$. 
- [[#Linear regression]]
- [[#k nearest method]]

#### Linear regression
The linear regression is a ==parametric approach==
$$Y | \underline{X} = \underline{x} \sim N(\underline{x}^{t} \underline{\beta}, \sigma^{2})$$

We estimate the parameter $\beta$ to predict Y
$$E[y | \underline{X} = \underline{x}] = β_{0} + β_{1}X_{1} + ..... + β_{p}X_{p}$$
####  k-Nearest Neighbors (k-NN) method
The k-nearest method is a ==non - parametric approach== 
$$\hat{f}(\underline{x}) = \widehat{E}[y|\underline{X} = \underline{x}] = Aug(y_{i}| x_{i} \in N_{k}(\underline{x}))$$
- Aug is the empirical mean
- $N_{k}$ neighborhood of $\underline{x}$ of the size of k 
This approach has been extended to kernel-based methods
![[K-nearest_for example.png]]


## Classification Setting
In classification, Y is categorical with k categories. A classifier assign a class to an object $\underline{x} \to y(\underline{x})$. 

Can be analyzed under various aspect:
- [[#Cost / loss function]]
- [[#Type of classification methods]]
	- [[#Logistic regression]]
	- [[#K-nearest neighborhood classifier]]
- [[#Bias - variance trade-off]] 


### Cost / loss function
The loss function is now defined by a $k \times k$ table. 

In the case of binary class is used **0 - 1 loss table**:

![[Screenshot 2025-02-27 at 09.23.34.png]]
This can be summarized has:
$$L(.) = \begin{cases} \ 1 \ \ if \ \ y_{i} \neq \widehat{y}_{i} \\ \ 0\ \ if \ \ y_{i} = \widehat{y}_{i} \end{cases}$$

- 1 if the actual output is different from the predicted output (I made a mistake) so ==add 1 to the loss==
- 0 if the actual output is the same from the predicted one, ==no addition to the loss==

**Expected 0-1 loss**
The mean of the the L(.) function represent the expected 0-1 loss:
$$E[L(y,y(\underline{x}))] = \frac{1}{n} \sum_{i=1}^{n}l(y_{i} \neq \widehat{y}_{i} )$$

Consider a fixed x:
- if x is predicted to class 0 $y(\underline{x} = 0)$, then:
$$
\begin{align}
E[L(y,y(\underline{x})) = & L(0,0)p(0|\underline{x}) + L(1,0)p(1|\underline{x}) \\
\text{So from the 0-1 loss table  } L(0,0) = 0 & \text{ and }  L(1,0) = 1 \text{ the result is:} \\ 
= & p(1|\underline{x})
\end{align}
$$
- If x is predicted to class 1 $y(\underline{x} = 1)$, then:
$$
\begin{align}
E[L(y,y(\underline{x})) = & L(0,1)p(0|\underline{x}) + L(1,1)p(1|\underline{x}) \\
\text{So } L(0,1) = 1 & \text{ and }  L(1,1) = 0 \text{ the result is:} \\ 
= & p(0|\underline{x})
\end{align}
$$

> [!NOTE]
> $p(1|x)$ rappresent the conditional probbaility that he observation x is in  class 1
>
>

#### Bayes classifier
The ==optimal classifier== is the bayes classifier that ==assigns each observation to the most likely class== (conditional probability) ==given its predictor values==:

Assign $\underline{x}$ to class 1 if 
$$
\begin{align}
p(0|\underline{x})< p(1|\underline{x}) & \text{ the conditional prob of 1 is higher }\\ 
1- p(1|\underline{x})< p(1|\underline{x}) & \text{ Note that: }p(0|\underline{x}) = 1- p(1|\underline{x}) \\
1- p(1|\underline{x}) - p(1|\underline{x}) < 0 &\\
1- 2p(1|\underline{x}) < 0 &\\
2p(1|\underline{x}) > 1 &\\
p(1|\underline{x}) > 0,5 &
\end{align}$$

In general, with more than 2 classes, then the bayes classifier assign $\underline{x}$ to a class 
$$j = \arg \max (p(i|\underline{x}))$$

**Bayes decision boundaries**
All $\underline{x}$ such that $p(1|\underline{x}) = 0,5$ (green line) \[p-1 manifold in the p-dimensional space\]

![[Screenshot 2025-02-27 at 14.22.39.png|300]]

Where:
- $x_{1}$ = class 1
- $x_{2}$ = class 2

So different classifications methods can be seen as providing an approximation to the boundaries (red line)

**Bayes error rate**
The error lies in the fact that even if you choose the most likely class, there is still a probability that it is wrong.
$$BER = 1- E_{\underline{x}}(\max_{{j}} \ p(j|\underline{x}))$$
*Example*
$$
\begin{align}
p(0∣x​)=0.3 &  \\
p(1∣x​)=0.7 & \\
1−\max(0.3,0.7)=1−0.7=0.3 & \text{ is the probability of error}
\end{align}
$$
So we have:
- BER > 0: irreducible error
- BER = 0: No overlap between classes


![[Bayes error rate.png|600]]

#### Asymmetric classification
The 0 - 1 loss can be generalized to different misclassification cost, where $C_{0}$ and $C_{1}$ can be different. This can be helpfull when:
- One misclassification is more costly than the other 
- there are unbalanced classes

For a given input $\underline{x}$​, the **expected loss** of
- predicting class 0 is:
$$
E[L(y,0)] = C_{o} \cdot p(1|\underline{x})
$$
- predicting class 1 is:
$$
E[L(y,1)] = C_{1} \cdot p(0|\underline{x})
$$

So the bayes classifier is given by:
Predict class 1 if:
$$
\begin{align}
C_{0}​ \cdot p(1∣\underline{x}) < C_{1}​ \cdot p(0∣\underline{x}) & \\
C_{0}​ \cdot p(1∣\underline{x}) < C_{1}​ \cdot (1-p(1∣\underline{x})) &\\
C_{0}​ \cdot p(1∣\underline{x}) < C_{1}​ - C_{1}​ \cdot p(1∣\underline{x}) & \\
C_{0}​ \cdot p(1∣\underline{x}) + C_{1}​ \cdot p(1∣\underline{x}) < C_{1}​  & \\
(C_{0} + C_{1}​) \cdot p(1∣\underline{x}) < C_{1}​  & \\
p(1|\underline{x}) < \frac{C_{1}}{C_{1} + C_{0}} &
\end{align}
$$
$\frac{C_{1}}{C_{1} + C_{0}}$ is the new threshold for classification
### Assessing model accuracy

#### Confusion Matrix
To asses a classification model accuracy is used the confusion matrix.

Since a $\hat{p}(1 | \underline{x})$ classifier calculate:
![[Screenshot 2025-02-28 at 09.11.10.png|300]]
[[Riassunto statistica#Matrice di confusione]]

Metric achievable via confusion matrix:
- $\text{ACC} = \frac{TP + TN}{ALL}$
- $\text{TPR} = \frac{\text{TP}}{\text{TP} + \text{FN}} \quad \text{(sensitivity)}$
- $\text{FPR} = \frac{\text{FP}}{\text{TN} + \text{FP}} \quad \text{(1 - specificity)}$
- $\text{ER} = \frac{\text{FP} + \text{FN}}{\text{TP} + \text{TN} + \text{FP} + \text{FN}} \quad \text{(error rate)}$

#### Receiver operating Characteristic (ROC) curve
![[ROC_curve.png|300]]

We plot (FPR, TPR) across thresholds $t \in [0,1]$, i.e. different value of $C_{0}, C_{1}$
$$
t^{*}=\frac{​C_{1}}{C_{0}+C_{1}}​​
$$

The closes the curve is to the optimal point, the best the classifier $(\hat{p} ( 1|\underline{x}))$ 
==We chose the classifier with the largest AUC== with a curve that is closest to the optimal point for the range of values of the thresholds (i.e. $C_{0}$) that one is interested in. 

**Relation between C and t**
Given $p(1|\underline{x}) > t$, assign the observation to the class 1:
- $t^{*} = 0.5$: $C_{0}$ and $C_{1}$ no cost asymmetry
- $t^{*}<0.5$: $C_{0}​>C_{1}​$ false negatives are more costly
	- You **lower the threshold** to reduce FN  
	- The classifier predicts class **1 more often**  
	Therefore:
	- TPR increases (more true positives)
	- **FPR increases** (more false positives)
- $t^{*}>0.5$: $C_{0}​<C_{1}​$ false positives are more costly
	- You **raise the threshold** to reduce FP  
	- The classifier predicts class **1 less often**  
	Therefore:
	- TPR decreases
	- FPR decreases

### Type of classification methods 
They will provide an estimate of $p(1|\underline{x}) - \hat{p}(1|\underline{x})$ - or more generally $$P(Y=j|\underline{X} = \underline{x}), \ \ j = 1, ..., c \text{ number of class}$$
There are several methodologies used in classification as:
- [[#Logistic regression]]
- [[#K-nearest neighborhood classifier]]

#### Logistic regression 
The logistic regression predict a label thought a parametric, supervised approach 


*Example 1*:
Predict medical condition of a patient arriving at emergency based on their symptoms. Assume that the response y can take only 3 possible values:
$$
y = 
\begin{cases} 
1 \ \ \text{if stroke} \\
2 \ \ \text{if drug overdose}\\
3  \ \ \text{if epileptic seizure}
\end{cases}
$$
Linear regression might work but it has two main problem:
- y is a qualitative so we cannot implying an ordering to the value of the response, becuse it will change with the range of the value
- We are also implying an unequal difference between the different feature (in this case is 1 but at the same time could be 3 depending on the dummy that we have chosen)

##### Binary Logistic Regression

![[Screenshot 2025-03-03 at 20.18.48.png|300]]

*Example 2*:
Lets consider only a binary response:
$$
y = 
\begin{cases} 
0 \ \ \text{if stroke} \\
1 \ \ \text{if drug overdose}\\
\end{cases}
$$

In a binary logistic regression y is binary $y = \{0,1\}$ and it has a [[Probability#Distribuzione binomiale Binomial Distribution|bernulli distribution]] with a function:
$$
f(y) = \begin{cases} 
p \ \ \ y = 1 \\
1-p \ \ \ y = 0\\
\end{cases}
$$
So we can ==model the prediction== as:
$$
\begin{align}
E[Y | \underline{X} = \underline{x}] =&\  0 * p(0|\underline{X} = \underline{x}) + 1*p(1|\underline{X} = \underline{x}) \\
=& \  p(1|\underline{X} = \underline{x}) 
\end{align}
$$
==!Important==, if we were in a regression setting we could make the following assumption, indeed we need to discretize the logits

From the model we ==make the following assumption== (linear regressions assumption):
$$
E[Y | \underline{X} = \underline{x}] = p(1|\underline{X} = \underline{x})  = \beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p} = \underline{\beta^{t}} \cdot \underline{x_{t}}
$$
==Then we discretize the probability ==
	The left part represent a discrete probability $[0,1]$, but the right part represent a continuous probability $(-\infty, \infty)$, creating a sort of issues (because linear regression might have values that are below 0). 
	To avoid this problem, we ==must model p(X) using a function that gives outputs between 0 and 1== for all values of X.
$$
\begin{align}
g(p(1|x)) \in (-\infty, \infty) \\
\text{or equivently} \\
p(1|x) = g^{-1}(\underline{x}) \in (-\infty, \infty)
\end{align}
$$
In logistic regression, we use the **logistic function (sigmoid function)**: 
$$
p(1|x) = \frac{e^{\beta^{t}\underline{x}}}{1 + e^{\beta^{t}\underline{x}}} = \frac{e^{\beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}}}{1 + e^{\beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}}} \in (0,1)
$$
*Example*
For one covariate 
$$
p(1|x) = \frac{e^{\beta_{0} + \beta_{1}\underline{x}}}{1 + e^{\beta_{0} + \beta_{1}\underline{x}}}
$$
![[Rplot02.png|300]]

**Interpretation of the coefficient**
The odds 
$$
\begin{align}
\frac{p(1|\underline{x})}{1-p(1|\underline{x})} & = \\
&=  \frac{\frac{e^{\beta^{t}\underline{x}}}{1 + e^{\beta^{t}\underline{x}}}}{ 1 - \frac{e^{\beta^{t}\underline{x}}}{1 + e^{\beta^{t}\underline{x}}}}\\
& = \frac{\frac{e^{\beta^{t}\underline{x}}}{\cancel{1 + e^{\beta^{t}\underline{x}}}}}{\frac{1}{\cancel{1 + e^{\beta^{t}\underline{x}}}}} \\
& = \frac{e^{\beta^{t}\underline{x}}}{1} = e^{\beta^{t}\underline{x}}
\end{align}
$$
the odds can take any value between 0 and ∞, where values close to 0 indicate very low probabilities of default, while values close to ∞ indicate very high probabilities of default.

Example:
	On average 1 in 5 people with an odds of 1/4 will default, since p(X) = 0.2 implies an odds of $\frac{0.2}{1−0.2} = \frac{1}{4}$

Log-odds (logit) 
$$ 
log\left(\frac{p(1|\underline{x})}{1-p(1|\underline{x})}\right)= log(e^{\beta^{t}\underline{x}}) = \beta^{t}\underline{x} = \beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}
$$
The logit function maps probabilities from (0,1) to the entire real number range $(-\infty, \infty)$. It is not the inverse of the sinusoid, but rather the inverse of the logistic function. In the formula A one-unit increase in $x_{j}$ will increase a change in the log-odds of y in of $b_{j}$, while keeping all the other variable fixed to a constant value.
So a logistic regression model is defined as follows:
$$
\begin{align}
&Y|\underline{X} = \underline{x} \sim \text{Bernulli}(p(1|\underline{x})) \text{ but this is not a real assumption} \\
&\text{From here is the real assumption} \\
& p(1|\underline{x}) = \frac{e^{\beta^{t}\underline{x}}}{1 + e^{\beta^{t}\underline{x}}} \text{ or equivantly } log\left(\frac{p(1|\underline{x})}{1-p(1|\underline{x})}\right)= log(e^{\beta^{t}\underline{x}}) = \beta^{t}\underline{x}
\end{align}
$$
**Parameter estimation**
To estimate $\beta$ from data is via maximizing the **likelihood**:
Given the  training data: $(\underline{x}_{i}, y_{i}), \ \ i = 1,...,n$ :
1) The likelihood of a logistic is the likelihood of a ==Bernoulli or binomial==: 
$$
f(y) = \begin{cases} p \ \ y=1 \\ 1-p \ \ y = 0 \end{cases} = p^{y}(1-p)^{1-y}
$$
2) ==Compute the likelihood== as the product of all these probabilities:
$$
L(\underline{\beta}) = \prod_{i = 1}^{n}p(1|\underline{x}_{i})^{y_{i}}(1-p(1|\underline{x}_{i}))^{1-y_{i}} 
$$
3) ==Compute the log-likelihood:==
$$
\begin{align}
l(\underline{\beta}) &= \sum_{i = 1}^{n} \log(p(1|\underline{x}_{i})^{y_{i}}(1-p(1|\underline{x}_{i}))^{1-y_{i}}) \\
&=\sum_{i = 1}^{n}[y_{i} \log(p(1|\underline{x_{i}}))+(1 - y_{i}) \log(1-p(1|\underline{x}_{i}))] \\
&\text{Looking to the note down}: \\
&= \sum_{i = 1}^{n}[y_{i}(\beta^{T} x_{i} - \log (1 + \beta^{T} x_{i})) - (1-y_{i})\log(1 + e^{\beta^{T}x_{i}})]  \\
&= \sum_{i = 1}^{n}[y_{i}\beta^{T} x_{i} - y_{i}\log (1 + \beta^{T} x_{i})) - \log(1 + e^{\beta^{T}x_{i}}) +y_{i} \log(1 + e^{\beta^{T}x_{i}})]  \\
&= \sum_{i = 1}^{n}[y_{i}\beta^{T} x_{i} \cancel{ - y_{i}\log (1 + \beta^{T} x_{i}))} - \log(1 + e^{\beta^{T}x_{i}}) \cancel{+y_{i} \log(1 + e^{\beta^{T}x_{i}})}]  \\
&= \sum_{i = 1}^{n} y_{i}\underline{\beta^{t}}\underline{x}_{i} - \log(1 + e^{\beta^{t}\underline{x}}) \text{ function of } \beta 
\end{align} 
$$
Note that for the properties of the logarithms:
$$
\log\left(\frac{b}{a}\right) = \log(b) - \log(a)
$$
In our case:
$$
\begin{align}
\log (p(1 | x_{i})) &= \log \left(\frac{e^{\beta^{T} x_{i}}}{1 + e^{\beta^{T} x_{i}}}\right) \\
&= \log (e^{\beta^{T} x_{i}}) - \log (1 + \beta^{T} x_{i}) \\ 
&= \beta^{T} x_{i} - \log (1 + \beta^{T} x_{i})
\end{align}
$$
$$
\begin{align}
\log \left( 1 - p(1 | x_{i}) \right) &=
\log \left( 1 - \frac{e^{\beta^T x_{i}}}{1 + e^{\beta^T x_{i}}} \right) \\
&=
\log \left( \frac{1}{1 + e^{\beta^{T} x_{i}}} \right) \\
&= \log(1) - \log \left( 1 + e^{\beta^{T} x_{i}} \right) \\
&= - \log \left( 1 + e^{\beta^{T} x_{i}} \right) 
\end{align}
$$
In r will be found the value that maximize the $\beta$ 

4) The optimization is to find $\beta$ that ==maximum the log-likelihood derivative==

$$
\begin{align}
\frac{\partial \ell (\beta)}{\partial \beta_j} &=
\sum_{i=1}^{n} y_i x_{ij} - \frac{e^{\beta^T x_i}}{1 + e^{\beta^T x_i}} x_{ij}\\
&= \sum_{i=1}^{n} y_i x_{ij} - p(1 | x_i) x_{ij} = 0, \quad j = 0, 1, \dots, p
\end{align}
$$
There is no closed expression for the solution (unlike [[Linear regression]] where $\hat{\underline{\beta}} = (X^{t}X)^{-1}X^{t}\underline{y}$), so the equations can only be solved numerically.

The function glm in R used the iteratively the weighted least square approximation for solving the equation (adaptation of newton - Rampson)

At the end we will get:
1) $\hat{\beta}_{0}, \hat{\beta}_{1}, ..., \hat{\beta}_{p}$  estimate of $\underline{\beta}$
2) Similar to the linear regression you also get the p-value:
	$H_{0}:\beta_{j} = 0, \ \ \ H_{1}:\beta_{j}≠0$
3) $\widehat{p(1|\underline{x})} = \frac{e^{\hat{\beta^{t}}\underline{x}}}{1 + e^{\hat{\beta^{t}}\underline{x}}} \to \text{classify } \underline{x} \ \ to \ \ 1 \ \ or \ \ 0$ by setting the threshold or we can calculate the ROC curve with test data

###### Multinomial Logistic Regression (Da non fare)
In case the classify response has more than two classes it is possible to extend the two-class logistic regression approach to the setting of K > 2 classes. 
1) select a single multinomial class to serve as the baseline
2) Select the Kth logistic class for this role
3) Replace the the model with $$ Pr(Y = k|X = x) = \frac{e^{\beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}}}{1 + \sum_{lp=1}^{K-1} e^{\beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}}} \text{for } k = 1,...,K-1$$ and the class K that is used like a baseline expressed as: $$ Pr(Y = K|X = x) = \frac{1}{1 + \sum_{lp=1}^{K-1} e^{\beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}}}$$
Repeating the odd formula of the logistic regression we obtain that
$$
\frac{Pr(Y = k|X = x)}{1- Pr(Y = k|X = x)}= \frac{Pr(Y = k|X = x)}{Pr(Y = K|X = x)}
$$
So that the log-odd (logit) is:
$$
ln(\frac{Pr(Y = k|X = x)}{Pr(Y = K|X = x)}) = \beta_{0} + \beta_{1}x_{1} + ... + \beta_{p}x_{p}
$$
The implementation of the logisstic regression is done through the [[softmax]] coding. In the softmax coding, rather than selecting a baseline class, we treat all K classes symmetrically, and assume that for $k = 1, \dots, K$,

$$
Pr(Y= k|X= x) = \frac{e^{\beta_{k0} + \beta_{k1} x_1 + \dots + \beta_{kp} x_p}}{\sum_{l=1}^{K} e^{\beta_{l0} + \beta_{l1} x_1 + \dots + \beta_{lp} x_p}}
$$

Thus, rather than estimating coefficients for \( K-1 \) classes, we actually estimate coefficients for all \( K \) classes. It is not hard to see that as a result of (4.13), the log odds ratio between the \( k \)th and \( k' \)th classes equals  

$$
\log \frac{Pr(Y= k|X= x)}{Pr(Y= k'|X= x)} = (\beta_{k0}- \beta_{k'0}) + (\beta_{k1}- \beta_{k'1})x_1 + \dots + (\beta_{kp}- \beta_{k'p})x_p.
$$

##### K-nearest neighborhood classifier 
The K-nearest neighborhood classifier (KNN) is a non-parametric approach, for each fixed K (positive integer 1,...,n) there is a classifier.
training data: $(\underline{x}_{i}, y_{i}), \ \ i = 1,...,n$ 
I would like to estimate the probability of $P(Y = j | \underline{X} = \underline{x_{0}})$ with ($\underline{x_{0}}$) a realisation of $\underline{X}$, is estimated as follows:
1) Identify the K training points that are the *closest* (depends on the metrics) to $\underline{x_{0}}$. Denotes this with $N_{k}(\underline{x}_{0})$. 
2) i approximate $\widehat{P(Y = j | \underline{X} = \underline{x_{0}})}$  with the percentages of observation that belong to the neighborhood $$\widehat{P(Y = j | \underline{X} = \underline{x_{0}})} = \frac{1}{K}\sum_{i \in N_{k}(\underline{x}_{0})}^{} I(y_{i} = j) \ \ j = 1,...,c $$
	I is an indicator function that count the observation that belongs to the neighborhood
3) Classify $\underline{x}_{0}$ to the class with the largest estimate probability (threshold = 0,5)

![[Screenshot 2025-03-03 at 19.07.39.png]]

![[KNN_decision_surface_animation.gif|400]]
###### Choice of the k
The selection of K is essential for the method's performance, as it is closely linked to the bias-variance trade-off:
- A **small K** increases the risk of **overfitting**, as the model closely follows the training data. 
- A **large K** may lead to **underfitting**, as the model smooths out patterns and loses sensitivity to local structures.

Example
Imagine you want to classify a new fruit as either an **apple** or an **orange** based on a dataset of labeled fruits.
- **If K = 1 (small)**, the model looks only at the **closest neighbor**. If that neighbor is an orange, it classifies the new fruit as an orange. This makes the model highly sensitive to noise, leading to **overfitting**.
- **If K = 10 (large)**, the model considers the **10 nearest neighbors** and assigns the most common label. This makes predictions more stable but may overlook local details, causing **underfitting**.
The best K is found through testing, typically choosing values like K=3,5 or 7 for a good bias-variance trade-off.

![[Screenshot 2025-03-03 at 19.13.01.png]]

I order to find the optimal K (the optimal trade off between bias and variance) is to calculate the error rate on some test data (because they are unseen during the training session)
1) Calculate probabilities on a test data for a range of value of K 
2) Use the response values of the test data to calculate the error rate 
3) choose the K that minimize the error rate

![[Screenshot 2025-03-03 at 19.22.12.png|300]]

The U shape means that at the 
- SX the model is too simple so it do
- DX the model is becoming more giggly, so it perform in the training data but not in the test data
The optimum exist in the minimum of the test data, where is also closed to the dotted line that represent the bayes error rate 
