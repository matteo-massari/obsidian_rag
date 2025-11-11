# Discriminant methods
In the book is the generative models for classification

==Discriminant methods are parametric classification methods== (LDA, QDA, ecc) for categorical methods (y: categorical, k: class). 
Discriminant analysis methods are an alternative to k-nn and logistic regression:
They are an alternative to the logistic regression because:
- there is a substantial separation between class, the parameter estimates for the logistic regression model are surprisingly unstable.
- If the distribution of the predictors X is approximately normal in  of the classes and the sample size is small. (more accurate than logistic regression.)
- the response variable can be easily extendable to more variable

As any classification model, the main objective is to estimate the posterior probability:
$$
P(Y=j | \underline{X} =\underline{x}), \ \ j = 1,...,k
$$
There probabilities are estimated "indirectly", by approximating:

Distribution of $\underline{X}$ of conditional on $Y=j$
$$
f_{j}(\underline{x}) = 
\begin{cases} 
P(\underline{X} = \underline{x}|Y =j) \quad \underline{x} \ \ \text{discrete}\\
f_{j}(\underline{x}) \text{ density } \quad \underline{x} \ \ \text{continuos}
\end{cases}
$$
Via bayes theorem, i do my estimation with the definition of conditional probabilities:

**Discrete formulation**
$$
P(Y=j | \underline{X} =\underline{x}) =  \frac{P(\underline{X} = \underline{x}|Y =j)P(Y =j)}{P(\underline{X} = \underline{x})}
$$
Because the K form a partition and the object belongs to 1 and only one of this classes the denominator can be written as
$$
P(Y=j | \underline{X} =\underline{x}) = \frac{P(\underline{X} = \underline{x}|Y =j)P(Y =j)}{\sum_{i=1}^{K}P(\underline{X} = \underline{x}|Y =j)P(Y =j)}
$$
**density formulation**
$$
P(Y=j | \underline{X} =\underline{x}) = \frac{f_{j}(\underline{x})\pi_{j}}{\sum_{i = 1}^{K}f_{j}(\underline{x})\pi_{j}}
$$
where:
- $\pi_{j} = P(Y =j)$ prior probability 
- $P(Y=j | \underline{X} =\underline{x})$ Posterior probability that an observation X = x belongs to the kth clas

The discriminant analysis method they will focus on estimating $f_{j}(\underline{x})\pi_{j}$ so that indirectly obtain the posterior probability. Then to make decision to the classification, we will assign $\underline{x}$ to the class that maximize (has the best probabilities):
$$
\begin{align}
&P(Y=j | \underline{X} =\underline{x}),  \ j = 1,...,k \ \ \\&or\\ \ \  &f_{j}(\underline{x})\pi_{j}, \  j = 1,...,k
\end{align}
$$
The denominator is the sum of all classes so it will not change with che class or  affect which class has the highest probability. So i really need just the class that maximize the numerator $P(\underline{X} = \underline{x}|Y =j)P(Y =j) \text{ or } f_{j}(\underline{x})\pi_{j}$

A simple visualization for p = 1:
- $f_{1}(x)$ = distribution of probabilities to have a disease
- $f_{2}(x)$ = distribution of probabilities to don't have a disease
In order to make a decision i also have to know $\pi_{1}, \pi_{2}$. In this example we have $\pi_{1} = \pi_{2}$ so the probability of get one of the two disease are equal

![[Pasted image 20250318142233.png|300]]

- On the left $f_{1}(x) > f_{2}(x)$ classify x to class 1
- On the left $f_{2}(x) > f_{1}(x)$ classify x to class 2

But if  $\pi_{1} < \pi_{2}$ than the decision point (green), shift to the left and i have to estimate agin the two distribution
![[Pasted image 20250318142424.png|300]]

Due the possible type of proportion of the $\pi_{j}$, there are two ways of estimation:
1) Equal for all classes, 
$$
\hat{\pi_{j}} = \frac{1}{K}
$$
two classe are equally distributed 
2) Finding through the data
$$\hat{\pi_{j}} = \frac{n_{j}}{n}$$
- $n_{j}$ number of observation from class j
- n = total number observation

## Type of discriminant analysis
The Bayes classifier assigns an observation x to the class with the highest value of $p_k(x)$, and it has the lowest possible error rate compared to all other classifiers. (This is true only if all the terms in density formulation are correctly specified.) Therefore, if we can estimate $f_k(x)$, we can use it in equation to approximate the Bayes classifier. There are three classifiers that use different estimates of $f_{j}(x)$ in *density formulation* to approximate the Bayes classifier:
- [[#Linear discriminant Analysis]]
- [[#Quadratic discriminant Analysis]]
- [[#Naive Bayes classifier]]
Discriminant analysis methods distinguish themselves on the assumption they make on  $f_{j}(x)$:

### Linear discriminant Analysis
#### p = 1
Analysis LDA for p = 1 (one-predictor)
To estimate $f_{j}(x)$, we make two assumption that
1) X conditional on $y=j$ is gaussian ([[Probability#Normal distribution|normal distribution]])
$$
f_{j}(x) = \frac{1}{\sqrt[]{2\pi\sigma_{j}}}\exp\{\frac{-1}{2\sigma_{j}^{2}}\left(x-\mu_{j}\right)^{2} \} 
$$
2) Homoskedasticity $\sigma^{2}_{1} = \sigma^{2}_{2} = ... = \sigma^{2}_{k} = \sigma^{2}$.

Due two this assumption it can have different mean (class dependent mean, but it must have the same variance).

The first two assumption lead to a linear decision surface. So the method is called linear discriminat analysis

##### Linear decision line
**Why linear?**
plugging $f_{j}(x)$ in the bayes formula:

$$
P(Y=j | \underline{X} =\underline{x}) = \frac{ \cancel{\frac{1}{\sqrt[]{2\pi\sigma_{j}}}}\exp\{\frac{-1}{2\sigma_{j}^{2}}\left(x-\mu_{k}\right)^{2} \}\pi_{k}  }
{\sum_{}^{} \cancel{\frac{1}{\sqrt[]{2\pi\sigma_{j}}}}\exp\{\frac{-1}{2\sigma_{j}^{2}}\left(x-\mu_{j}\right)^{2} \pi_{j}\} }
$$
- $\pi_{k}$ prior probability that an observation belong to the $k_{th}$ 

The bases classifier involves assigning an observation X = x to the class for which class $P(Y=j | \underline{X} =\underline{x})$ is larger. So maximizing $P(Y=j | \underline{X} =\underline{x})$ is equivalent to finding j that maximize:
$$
\exp \left\{\frac{-1}{2}\left(\frac{x-\mu_{j}}{\sigma_{j}}\right)^{2} \right\}\pi_{j}
$$

Due to the difficulty of the computation, we can apply the logarithm and obtain a form that is easier to manipulate  
$$
\delta_i (x) = -\frac{1}{2} \left( \frac{x - \mu_i}{\sigma} \right)^2 + \log (\pi_j)
$$

$$
= -\frac{1}{2} \frac{x^2}{\sigma^2} + \frac{x \mu_i}{\sigma^2} - \frac{1}{2} \frac{\mu_i^2}{\sigma^2} + \log (\Pi_j)
$$
So:
$$
\delta_{i} (x) \quad (?) \quad = -\frac{1}{2} \frac{\mu_i^2}{\sigma^2} + \log (\Pi_j) + x \frac{\mu_i}{\sigma^2}
$$

Define:
- $(\beta_0)_i = -\frac{1}{2} \frac{\mu_i^2}{\sigma^2} + \log (\Pi_j)$
- $(\beta_1)_j = \frac{\mu_i}{\sigma^2}$

Poiché la funzione risultante è lineare:
$$
\delta_1 (x) = \delta_2 (x) \text{ è una funzione lineare.}
$$

#####  Parameter Estimation
The LDA method approxiamte the bayes classifier by plugging the estimate for $\pi_{k}, \mu_{1},...,\mu_{k}$ and $\sigma^{2}$. These parameter are estimated by the maximum likelihood

*Mean*:
$$
\hat{\mu_{j}} = \frac{1}{n_{j}}\sum_{i:y_{i}=j}^{}x_{i}
$$
*Pooled variance* (when the variance is equal for every class)
$$
\hat{\sigma}^{2} = \sum_{j = 1}^{k} \frac{(n_{j}-1)s^{2}_{j}}{\sum_{}^{k}(n_{j}-i)} = 
\frac{\sum_{j=1}^{k} \sum_{}^{i:y_{i}=j}(x_{j}- \hat{\mu_{j}})^{2}}{n-k}
$$
- $s^2$ = empirical mean that i take from the data 
- n-k because is k means for k groups are estimated, so il lose degrees of freedom

*Prior probability* $\pi_{k}$:
$$
\hat{\pi_{k}} = \frac{n_{k}}{n}
$$
Evaluate the proportion of the observation in the k_th class, to the total of observation


#### p > 1 (multivariate LDA)
Generalization of the model in more dimensione (1 is a point, 2 is a line, 3 is a plane ecc ecc). 

The Multivariate LDA makes two assumption 
1) X conditional on $y=j$ is gaussian (Multivariate Gaussian Distribution $X\sim N(\mu, \Sigma)$)
$$
f_{j}(\underline{x}) = \frac{1}{(2\pi)^{\frac{k}{2}}*det(\Sigma_{j})^{\frac{1}{2}}}\exp \left\{    - \frac{1}{2}(\underline{x} - \underline{\mu_{j}})^{t} \Sigma_{j}^{-1}(\underline{x} - \underline{\mu_{j}}) \right\}
$$
- $\underline{\mu} = \begin{cases} \mu_{j} \\ \vdots  \\ \mu_{j} \end{cases}$ vector of the means
- $\Sigma_{j} \ \ p \times p$ covariance matrix

2) Homoskedasticity (equal variance) $\Sigma_{1}= ... = \Sigma_{k}$
![[Pasted image 20250318192543.png|300]]

In this case, $\text{Var}(X_1) = \text{Var}(X_2)$ and $\text{Cor}(X_1, X_2) = 0$, so the surface has a characteristic bell shape. If the predictors are correlated or have unequal variances, the bell shape becomes distorted (which, theoretically, is not possible in LDA). *Therefore, the positions of the means differ, but the shape of the distributions remains the same due to the assumption of homoscedasticity*.
![[Pasted image 20250318192657.png|300]]

##### Linear decision surface
As before, maximizing $P(Y = j | X = x)$ (the posterior probability) is equivalent to maximizing 
 $\delta_{j}(x) = \log (\Pi_{j} f_{j (x)})$. In this maximization is equal to plug-in the $f_{j}(\underline{x})$ function to the **density function** seen before. Indeed plug in the $f_{j}(\underline{x})$ in the reduced form that reveals that the Bayes classifier assigns an observation $X = x$ to the class for which:
$$
\begin{align}
\delta_{j} (x) &= \log (\Pi_{j} f_{j (x)})= \\
&= \log (\Pi_{j}) + \log \left( \frac{1}{(2\pi)^{d/2} |\Sigma|^{1/2}} \exp \left\{ -\frac{1}{2} (x - \mu_{j})^{t} \Sigma^{-1} (x - \mu_{j}) \right\} \right)\\
&\propto \log (\Pi_{j}) - \frac{1}{2} (x - \mu_{j})^{t} \Sigma^{-1} (x - \mu_{j})\\
&= \log (\Pi_{j}) - \frac{1}{2} x^{t} \Sigma^{-1} x - \frac{1}{2} \mu_{j}^{t} \Sigma^{-1} \mu_{j} + x^{t} \Sigma^{-1} \mu_{j} \\
&\propto \log (\Pi_j) - \frac{1}{2} \mu_j^t \Sigma^{-1} \mu_j + x^t \Sigma^{-1} \mu_j
\end{align}
$$
is largest. This is in the form of a vector / matrix

Can be derived the linear form as:
- $(\beta_{0})_{j} = \log (\Pi_{j}) - \frac{1}{2} \mu_{j}^{t} \Sigma^{-1} \mu_{j}$
- $(\beta_1)_j = \Sigma^{-1} \mu_j$
$$δ_{j}​(x)=β_{0j}​​+x^{T}β_{1j}​​$$
Poiché la funzione risultante è lineare:
$$
\delta_1 (x) = \delta_2 (x) \text{ è una funzione lineare.}
$$

As the  p = 1, we need to *estimate* $\mu_1, ..., \mu_K, \Sigma$ from data:

$$
\begin{align}
& \Rightarrow \delta_{1} (x), ..., \delta_{K} (x)\\
&\Rightarrow P(Y = i | X = x) = \frac{e^{\delta_i (x)}}{\sum_{i=1}^{K} e^{\delta_i (x)}}
\end{align}
$$
And classify x to the class j that maximize $P(Y=j | X = x) \text{ or }δ_{j}​(x)$
Poiché la decision surface è lineare:
$$
X : \delta_1 (x) = \delta_2 (x) \text{ è lineare.}
$$

#### LDA vs Logistic Regression

For simplicity, consider \( p=1, K=2 \).

- Log-odds for Logistic Regression:
$$
\log \left( \frac{P(Z=1 | X)}{P(Z=1 | X)} \right) = \beta_0 + \beta_1 x
$$

- Log-odds for LDA:
$$
\log \left( \frac{P(Z=1 | X)}{P(Z=1 | X)} \right) = \log \left( \frac{e^{s_2 C_x}}{e^{s_1 C_x}} \right) = \delta_2 (X) - \delta_1 (X) = \delta_0 + \delta_1 X
$$

The Differences:
- LDA makes assumptions on X conditional on $Y=j$.
- Different ways of estimating parameters. LDA is more stable when probabilities are close to 0 and 1.

### Quadratic Linear Analysis
The QDA has the same assumption of the LDA, but it relaxes the assumption of equal covariates, so it assume:
$$
X|Y \sim N_{p}(\mu_{j}, \Sigma_{j})
$$
- $\Sigma$ various covariance matrices- > one for each class
So the model is more flexible, at the expense of most more parameters. (bias-variance tradeoff)

Explanation from the book:
When there are p predictors, then estimating a covariance matrix requires estimating $p\frac{p+1}{2}$ parameters. QDA estimates a separate covariance matrix for each class, for a total of $K* p\frac{p+1}{2}$ parameters. With 50 predictors this is some multiple of 1,275, which is a lot of parameters. In contrast the QDA due to the different covariance is more flexible than the LDA that remain linear. 

#### Quadratic Decision Surface

As before,

$$
P(Y = j | x = x) = \frac{f_j(x) \pi_j}{\sum_{i=1}^{K} f_i(x) \pi_i}
= \frac{\frac{1}{(2\pi)^{p/2} \det(\Sigma_j)^{1/2}} e^{- \frac{1}{2} (x - \mu_j)^t \Sigma_j^{-1} (x - \mu_j)}}
{\sum_{i=1}^{K} \frac{1}{(2\pi)^{p/2} \det(\Sigma_i)^{1/2}} e^{- \frac{1}{2} (x - \mu_i)^t \Sigma_i^{-1} (x - \mu_i)}}
$$

Maximizing $P(Y = j | x = x)$ is equivalent to maximizing

$$
S_j(x) = -\frac{1}{2} \log (\det (\Sigma_j)) - \frac{1}{2} (x - \mu_j)^t \Sigma_j^{-1} (x - \mu_j)
$$

$$
= -\frac{1}{2} \log (\det (\Sigma_i)) - \frac{1}{2} \mu_j^t \Sigma_j^{-1} \mu_j 
+ x^t \Sigma_j^{-1} \mu_j - \frac{1}{2} x^t \Sigma_j^{-1} x
$$

Dove i termini possono essere identificati come:
- **Costante**: $-\frac{1}{2} \log (\det (\Sigma_i)) - \frac{1}{2} \mu_j^t \Sigma_j^{-1} \mu_j$ 
- **Lineare**: $x^{t} \Sigma_{j}^{-1} \mu_{j}$
- **Quadratico**: $-\frac{1}{2} x^t \Sigma_j^{-1} x$

![[LDA_QDA.png|300]]

The two methods have a different number of parameters (bias variance trade-off explanation):
- LDA:
$$
\mu_i, \mu_k, \Sigma \quad \frac{k \, p_t \, p(CP+1)}{2}
$$
- QDA:
$$
\mu_i, \mu_k, \Sigma_k, ..., \Sigma_K \quad \frac{k \, p_t \, k \, p(CP+1)}{2}
$$

Example: K=2, p=50
- **LDA:** 1375
- **QDA:** 2650

### Naive Bayes classifier
y is Categorical, K classes. It belongs to the family of discriminant analysis methods.
The assumptions on $f_{j}(\underline{x})$ (is between  LDA and QDA):

$$
f_{j}(\underline{X} = x_{1},...,x_{p}) = \prod_{l=1}^{p}f_{je}(x_{e})
$$
with:
- $f_{je}$ the distribution of $x_{E}$ on y = j

So the assumption is that $x_{1},...,x_{p}$ are independent conditional on the class. if $\underline{X}$ are discrete variable then:
$$
P(X_{1} = x_{1},..., X_{p} = x_{p} | Y = j) = P(X_{1} = x_{1}|Y = j)...P(X_{p} = x_{p}|Y = j)
$$
This classifier is a special case of bayesian networks
#### Gaussian Naive bayes
Assumption 
1) $\underline{X} |Y=j \sim MVN_{p}(\underline{\mu_{j}}, \Sigma_{j})$ (same assumption of QDA)
2) $x_{i} \bot x_{e} | Y = j$ conditional independence assumption.
Under a Gaussian assumption:
$$
w \bot z \ \ \leftrightarrow  cov(w,z) = 0
$$
So the two assumption combined meant that
$$
\Sigma_{j} = begin 
$$
![[Screenshot 2025-03-24 at 15.09.03.png|300]]

SO compared with:
- LDA has variance that do not depend on j but off-diagonal non 0 value 
- QDA does have the same diagonal but non-zero off-diagonal 

#### Naive bayes decision surface 
$$
\begin{align}
P(Y = j \mid X = \mathbf{x}) &\propto \pi_j f_j(\mathbf{x}) \quad \text{dove } \mathbf{x} \in Y = j \Rightarrow \text{indipendenza condizionata} \\
&= \pi_j \prod_{e=1}^p f_{je}(x_e)\\
&= \pi_j \prod_{e=1}^p \frac{1}{\sqrt{2\pi} \, \sigma_{ej}} \exp\left( -\frac{1}{2} \left( \frac{x_e - \mu_{ej}}{\sigma_{ej}} \right)^2 \right)

\end{align}
$$

So the decision surface is given by
$$
\delta_j(\mathbf{x}) = \log(\pi_j) - \sum_{e=1}^p \log(\sigma_{ej}) - \frac{1}{2} \sum_{e=1}^p \left( \frac{x_e - \mu_{ej}}{\sigma_{ej}} \right)^2
$$
It results in a "simple quadratic form" which do not require the inversion of $\Sigma$ or $\Sigma_{j}$ 
#### Multinomial Naive bayes classifier

$X_{1},...,X_{p}$ is categorical (or if is continue i categorized them)
We consider $F_{j}(\underline{X}) = P(\underline{X} = \underline{x}|Y=j)$ in this case

**Parameter estimation**
y is binary, so has two classes, j = 1,2
$X_i$ is categorical and has $G_i$ categories
I want to the probability

$$
\begin{align}
P(Y=j|\underline{X} = \underline{x}) \propto P(Y=j)P(X_{1} = x_{1}|Y=j) * ... *P(X_{1} = x_{1}|Y=j)
\end{align}
$$
where :
- $P(Y=j)$
	- $\theta_{y1} = P(Y=1)$
	- $\theta_{y2} = P(Y=2)$
- $P(Y=j)P(X_{1} = x_{1}|Y=j)$ The possibility of Y=1 for every category 
	- $\theta_{111} = P(X_{1} = 1|Y=1)$
	- $\theta_{11G_{1}} = P(X_{1} = G_{1}|Y=1)$
	- $\theta_{121} = P(X_{1} = 1|Y=1)$
	- $\theta_{12G_{1}} = P(X_{1} = G_{1}|Y=1)$
- $P(X_{1} = x_{1}|Y=j)$
	- $\theta_{p11} = P(X_{1} = 1|Y=1)$
	- $\theta_{p1G_{1}} = P(X_{1} = G_{1}|Y=1)$
	- $\theta_{p21} = P(X_{1} = 1|Y=1)$
	- $\theta_{p2G_{1}} = P(X_{1} = G_{1}|Y=1)$
$X_{1}$ first index is the one of the predictor 
$G_{1}$ second is the index that i use for the class 
$x_{1}$ third index is the one that i use for the category of $x_{1}$ can take

This $\theta$ is the one that i have to learn by the data. So we have to do an estimation of $\underline{\theta}$ from the data under the assumption od global independence of parameter between predictors and of local independence for one predictor and between classes (so whatever is the estimate that i get from $\theta_{111}$ is independent to $\theta_{121}$).
So we need to estimate to estimate the distribution of $X_{1}|Y_{j}$ from data, separately for each predictor $x_{i}$ and class j

