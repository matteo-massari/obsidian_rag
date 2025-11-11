# Statistical learning
Statistical Learning seeks to identify and understand hidden patterns in data through the use of advanced algorithms and analysis techniques in order to make informed decisions and accurate predictions about future phenomena.

Example: 
	Wage data: wage of workers in US
	Objective: predict wage from a number of factors (search a sort of correlation between datas)

In general, we could formalize the problem as follows:
- $y$: response (or  dependent, outcome. ecc):
	- Numerical (regression problem)
	- Categorical (Classification problem)
- $\underline{x} = (x_{1}, ..., x_{p})^{t}$: predictors / features / covariants / independent variables. Are vector of p different predictors
- Training Data: is a dataset of n observations that we will use to 'instruct' or 'teach' our method how to extract information. The training dataset is composed of the pairs ${(x_1,y_1),...,(x_n,y_n)}$, where $x_i =(x_{i1},x_{i2},...,x_{ip})^T$ and $i = 1, ..., n$. is of size n, p+1

Let's assume an existing relation between **Y** and **X** :
$$y = f(\underline{x}) + \epsilon$$
- $f(\underline{x})$: fixed but unknown function of $x_1, ... x_p$ (systematic compound)
- $\epsilon$ random error term (stochastic component) (it have the proprieties of the error $E[\epsilon] = 0$)
The function that binds the input variable to the output variables is unknown so it must be estimated from the data.

## Estimating the function
Then Statistical learning is about estimating $\widehat{f}$ 

### Reasons
The two main reason are:
- [[#Prediction]]
- [[#Inference]] 

#### Prediction 
Predicting y when you only have observation of x
Given $\widehat{f}$ estimate of $f$, 
$$\widehat{{Y}} = \widehat{f}(\underline{x})$$
The assumption of the model are:
- $E[\epsilon] = 0$
- $Var(\epsilon) = \sigma^{2}$ 

The estimated model $\widehat{f}(\underline{x})$ can be a black box, so (with unsupervised learning) i can also obtain the result without knowing perfectly what it does. 

The accuracy of $\hat{f}$ as a prediction for y can be described by
$$
\begin{align}
MSE 
&= E[(y - \widehat{f}(\underline{x}))^{2} | \underline{X} = \underline{x}] \\ 
&= E[(f(\underline{x}) + \epsilon - \widehat{f}(\underline{x}))^{2} | \underline{X} = \underline{x}] \\ 
&= E[(f(\underline{x}) - \widehat{f}(\underline{x}) + \epsilon )^{2} | \underline{X} = \underline{x}] \\ 
&= E[(f(\underline{x}) - \widehat{f}(\underline{x}))^{2} | \underline{X} = \underline{x}] + E[\epsilon^{2} | \underline{X} = \underline{x}] + 2E[(f(\underline{x}) - \widehat{f}(\underline{x}))\epsilon | \underline{X} = \underline{x}] \\ 
&= E[(f(\underline{x}) - \widehat{f}(\underline{x}))^{2} | \underline{X} = \underline{x}] + var(\epsilon) \\ 
&= (f(\underline{x}) - \widehat{f}(\underline{x}))^{2} + var(\epsilon) \\
&= \text{reducible error } + \text{ irreducible error}
\end{align}
$$
Note that $2E[f((\underline{x}) - \widehat{f}(\underline{x}))*\epsilon = 0$ because $E[\epsilon] = 0$


This can be divided in 
- Reducible error $(f(\underline{x}) - \widehat{f}(\underline{x}))^{2}$: 
	$\widehat{f}$ will not be a perfect estimate of f and therefore will generate an error, which is reducible because one can potentially increase the accuracy of $\widehat{f}$ by using the most appropriate model.
- Irreducible error $var(\underline{\epsilon})$
	even under the assumption that we find a perfect estimate of $\widehat{f}$, our prediction will still have an error because Y is also a function of ε which, by definition, cannot be predicted using X , since ε may contain:
	- variables that are not used but could be useful in predicting Y
	- unmeasurable variations (such as the risk of an adverse drug reaction)

Note that the error is present only in the second member, which being an estimate may vary. The goal is to minimize the reducible error
![[Function rappresentation.png|300]]

#### Inference
The main aim is to understand how $\textbf{Y}$ is affected by changing the values of $\textbf{X}_{1} , ..., \textbf{X}_{p}$; so we will estimate $\widehat{f}$, but the goal will be to understand the relationship between $\textbf{Y}$ and $\textbf{X}$.

In the Area of inference will be explored (paradigm of inference): 
1) Which predictors are associated with the response 
2) what is the relationship between y and $x_j$ 
So we are searching for a method that is easily interpreted

### Type of function
To estimate $f$ is needed training data with observation $(\underline{x_{i}}, y_{i}) = i = 1,...,n$  with $(x_{i,1},...,x_{i,p})^{t}$ that contains the observation associated to a variable response in y
(aggiungere schema che mostra la relazione)

There are two type of approach: 
- [[#Parametric]]
- [[#Non Parametric]]

#### Parametric
We make the assumption about the functional form of $f$, $$f(\underline{\textbf{X}}) = β_{0} + β_{1}X_{1} + ..... + β_{p}X_{p}$$
So the parameters are unknown and statistical learning become a parameter estimation:
$$\hat{β_{0}}, \hat{β_{1}}, ..., \hat{β_{p}} \to \widehat{f}(\underline{x})$$ can be plugged in $f(\underline{\textbf{X}})$

Disadvantage: the function f may be far from the parametric family
Advantage: Interpretability
Note: Linear models are linear on the parameter so it can be plotted also as a linear model (or can be more and more complex as the spline model)

#### Non Parametric
There are no explicit assumption on the functional form of f. They estimate f by getting an approximation as possible to the data, without being too rough too wiggly. More flexible, but may have greater potential for overfitting 

- ADVANTAGES: by avoiding the assumption of a particular functional form for f , they can be accurately fitted to a wider range of possible shapes (they are more flexible).
- DISADVANTAGES: a very large number of observations is required to obtain an accurate estimate of f. Also, we will have to estimate a larger number of parameters leading to the phenomenon known as data overfitting: that is, they follow the data too closely in their deviation from the background function (noise). They can lead to such complicated estimates for f that it is difficult to understand how each individual predictor is associated with Y . Thus, they are not well suited for inference.

**Trade off**

| Parametric Method      | Non-Parametric Method       |
| ---------------------- | --------------------------- |
| Model interpretability | Prediction accuracy         |
| Good fit               | overfitting or underfitting |
| Parsimony              | black box                   |
We often prefer a simpler model that involves fewer variables than a black-box predictor that involves all of them.
