# Generalize Linear Model
Rielaborare
==The GLM are a family of model for general response type, count, continuos, categorical. All these models makes assumptions on the conditional distribution of y given $\underline{x}$. We will denigrate with $y_i$ the random variable== 

from the book:
A GLM is defined by specifying two components:
- ==Response== should be a ==member of the exponential family distribution== 
- ==Link function== describes ==how the mean of the response and a linear combination of the predictors are related==.
## GLM funzionamento 
The element of a General Linear Model are:
$$
y_{i} \sim f(y_{i}, \mu_{i}, \Phi)
$$

 $f$ = link function: is a function that link the model at the prediction. The $f$ has to be a member of the [[#exponential dispersion family]] (EDF), so that each choice of f and g corresponds to a GLM. 

**Example**

|     | Linear regression | Logistic regression                                        | Poisson regression |
| --- | ----------------- | ---------------------------------------------------------- | ------------------ |
| $f$ | Gaussian          | Bernulli                                                   | Poisson            |
| $g$ | Identity          | [[Statistical decision theory#Logistic regression\|logit]] | log                |
|     |                   |                                                            |                    |

**Example with linear regression**
$$
y_{i}= (y| \underline{X} = \underline{x})
$$
We assume that the response is a gaussian $y_{i} \sim N(\mu_{i}, \sigma^{2})$. The mean is $u_{i} = E[y_{i}] = \underline{x_{i}}^{t}\beta = \eta$, identifies as the linear predictor

### Exponential dispersion family
A density belongs to the EDF if it can be written as:
$$
f(y_{i}, \theta_{i},\Phi) = \exp \left\{ \frac{y_{i}\theta_{i} - b(\theta_{i})}{a_{i}(\Phi)} + c(y_{i}, \Phi) \right\}
$$
- $\theta_{i}$: ==canonical parameter==. It represent the location, so it ==determine the central value of the distribution== (ex: for the normal is the $\mu$). The choice of $\theta$ as a canonical parameter allows the distribution to be written in a standardized form
- $\Phi$: ==dispersion parameter==. It represent the scale, governs the variability of the distribution: 
	- greater the Φ, the wider the distribution; 
	- smaller the Φ, the narrower the distribution. (ex: for the normal distribution is the variance)
- $y_{i}:$ response observation 
We may define various members of the family by specifying the functions _a, b,_ and _c_ (these function are different for every distribution)

**Example Gaussian**
Gaussian belongs to EDP
$$
\begin{align}
f(y_{i}, µ_{i}, \sigma^{2}) &= \frac{1}{\sqrt[]{2 \pi \sigma^{2}}} \exp \{-\frac{1}{2} \frac{(y_{i}- µ_{i})^{2}}{\sigma^{2}}\} \\
&= \exp\{-\frac{1}{2} \frac{(y_{i}- µ_{i})^{2}}{\sigma^{2}} + \ln\left(\frac{1}{\sqrt[]{2 \pi \sigma^{2}}} \right)\} \\
&= \exp\{-\frac{1}{2} \frac{y_{i}^{2} -2y_{2}µ_{2}  + µ_{i}^{2}}{\sigma^{2}} - \frac{1}{2} \ln(2\pi\sigma^{2}\} \\
&= \exp\{\frac{y_{i} µ_{i} - \frac{µ_{i}}{2}}{\sigma^{2}}- \frac{1}{2} \frac{y^{2}_{i}}{\sigma^{2}} - \frac{1}{2}ln(2 \pi \sigma^{2})\}
\end{align}
$$
The gaussian distribution belongs to the EDF with these parameters
$$
\theta_{i}= \mu_{i}, \ \  \Phi_{i}= \sigma^{2}, \ \ b(\theta_{i}) = \frac{\theta^{2}_{i}}{2}, \ \ a_{i}(\Phi) = \Phi, \ \ c(y_{i}, \Phi) = -\frac{1}{2} \frac{y^{2}_{i}}{\sigma^{2}} - \frac{1}{2}ln(2 \pi \sigma^{2})
$$
We can retrive those parameter from the generic model

**Example Binomial**
$$
\begin{align}
f(y_{i}, n_{i}, \pi_{i}) &= \binom{n_{i}}{y_{i}} \pi_{i}^{y_{i}}(1-\pi_{i})^{n_{i}- y_{i}} \\
&= \exp\left\{y_{i}\ln(\pi_{i}) + (n_{i}- y_{i})\ln(1-\pi) + \ln\binom{n_{i}}{y_{i}} \right\} \\
&= \exp\left\{y_{i}\ln(\pi_{i}) + n_{i}\ln(1-\pi) - y_{i}\ln(1-\pi) + \ln\binom{n_{i}}{y_{i}} \right\} \\
&= \exp\left\{y_{i} \ln \left(\frac{\pi_{i}}{1-\pi_{i}}\right) + n_{i} \ln(1-\pi) + \ln \binom{n_{i}}{y_{i}} \right\}
\end{align}
$$
Binomial is a member of the EDF
$$
\begin{align}
\theta_{i}= \ln\left(\frac{\pi_{i}}{1-\pi_{i}}\right), \ \ \Phi = 1, \ \ b(\theta_{i}) =  -n_{i} \ln(1-\pi), \ \ a_{i}(\Phi) = \Phi, \ \ c(y_{i}\Phi) = \ln\binom{n_{i}}{y_{i}}
\end{align}
$$
From the two example the parameter $\Phi$ for the gaussian is fixed and for the binomial is free, this because the Poisson and binomial are one parameter families while the normal and gamma have two parameters

By computing the canonical parameter of the binomial (the mean or expected value), we can derive the logistic regression link function. ==This explains why we can model a binomial response with the logistic function==.

$$
\begin{align}
e^{\theta_{i}}&= \frac{\pi_{i}}{1-\pi_{i}}\\
(1-\pi_{i}) e^{\theta_{i}} &= \pi_{i} \\
\pi_{i} &= \frac{e^{\theta_{i}}}{1+e^{\theta_{i}}} \text{this is the logistic function}
\end{align}
$$
In fact, the value $π$ represents the probability of a dichotomous event (such as success or failure) and can be modeled using logistic regression
#### Connection between new and old parameter
**Example**
$y_{i}\sim \text{Binomial}(n_{i}, \pi_{i})$ 
$$
\begin{align}
&u_{i} = E[y_{i}] =n_{i} \pi_{i} = n_{i} \frac{e^{\theta_{i}}}{1 + e^{\theta_{i}}} \text{ Connection between } \theta_{i} \text{ and } µ_{i}\\
&Var(y_{i}) = n_{i}\pi_{i}(1-\pi_{i}) = µ \left(1- \frac{µ_{i}}{n_{i}}\right)= µ_{i} \frac{n_{i}-µ_{i}}{n_{i}}\\
\end{align}
$$
The variance depends on the mean $\mu_{i}$, indicating a natural heteroskedasticity. The variance demonstrates that if we model the mean $\mu_{i}$, we implicitly model the variance as well.

**Connection between $\sigma_{i}$ and $µ_{i}$** 
Can be proved via:
$$
\begin{align}
µ_{i} &= E[y_{i}] = b'\text{(first derivate)}(\theta_{i}) \\
Var(y_{i}) &= b''\text{(second derivate)}(\theta_{i})a_{i}(\Phi)\\
&=V(\mu_{i}) a_{i}(\Phi)
\end{align}
$$
Check with the gaussian and the binomial
1) Gaussian $b(\theta_{i}) = \frac{\sigma^{2}_{i}}{2}, \ \ \theta_{i} = \mu_{i} \ \ \Phi = \sigma^{2}$
2) Binomial $b(\theta_{i}) = n_{i}ln(1+e^{\theta_{i}}), \theta_{i}= ln\left(\frac{\pi_{i}}{1-\pi_{i}}\right)$ 

### Link function
Let us suppose we may express the effect of the predictors on the response through a linear predictor:  
$$\eta = \beta_0 + \beta_1 x_1 + \dots + \beta_p x_p = \mathbf{x}^T \boldsymbol{\beta}$$
The link function, $g$ describes how the mean response, $E[Y]=\mu$, is linked to the covariates (x) through the linear predictor:
$$
\begin{align}
g(µ_{i}) = \eta = \underline{x}_{i}^{t} \underline{\beta} \\
\text{with } µ_{i} = E[y|\underline{X} = x_{i}]
\end{align}
$$
The link function is necessary when µ cannot be directly modeled linearly (e.g., probabilities or counts).

Why the link function is usefull? (taken from the book)
	In principle, any monotone continuous and differentiable function will do, but there are some convenient and common choices for the standard GLMs. In the Gaussian linear model, the identity link, $\eta = \mu$, is the obvious selection, but another choice would give $y = g^{-1}(x^T\beta) + \varepsilon$. This does not correspond directly to a transform on the response: $g(y) = x^T\beta + \varepsilon$ as, for example, in a [[Box-Cox type transformation]]. In a GLM, the link function is assumed known, whereas in a single index model, $g$ is estimated. For the Poisson GLM, the mean $\mu$ must be positive, so $\eta = \mu$ will not work conveniently since $\eta$ can be negative. The standard choice is $\mu = e^{\eta}$ so that $\eta = \log \mu$, which ensures $\mu > 0$. This log link means that additive effects of $x$ lead to multiplicative effects on $\mu$. For the binomial GLM, let $p$ be the probability of success and let this be our $\mu$ if we define the response as the proportion rather than the count. This requires that $0 \leq p \leq 1$. There are several commonly used ways to ensure this: the logistic, probit, and complementary log-log links.

Consider the case of Bernoulli
$$
\begin{align}
&y_{i} \sim Bernulli(\pi_{i}) \ \ \text{with} \ \ \pi_{i} = p(1|\underline{X} = x_{i}) = µ_{i} \\
&u_{i}\in (0,1)\\
&\eta \in (-\infty,\infty)\\
&g:(0,1) \to (-\infty,\infty) \text{ or equivalent } g^{-1}:(-\infty,\infty) \to (0,1)
\end{align}
$$

Two of the common choices are obtained by considering the CDF of an absolutely continuous random variable 

The three popular choices:

**Probit link**
Given a probit regression where  $Y_i \sim \text{Bernoulli}(\mu_i)$, and the link function is $g = \Phi^{-1}$,  
we use the cumulative distribution function (CDF) of the standard normal distribution:
$$\Phi(z) = \mathbb{P}(Z \leq z), \quad Z \sim \mathcal{N}(0, 1)$$
The inverse of this function defines the link function:
$$g = \Phi^{-1} : (0,1) \to (-\infty, \infty)$$
This function maps a probability $µ \in (0,1)$ to a real number $\eta \in \mathbb{R}$,  
allowing us to express the mean response on the linear predictor scale:
$$g(\mu_i) = \eta_i = \mathbf{x}_i^\top \boldsymbol{\beta}$$


**Logit function**
3) Consider now the logistic distribution: $$ f(y,µ,\sigma^{2}) = \frac{exp[\frac{y-µ}{\sigma}]}{\sigma^{2}[1 + exp(\frac{y-µ}{\sigma})]} \ \ -\infty < y < \infty $$
	Setting $\mu = 0, \ \sigma^{2}=1$ $$ F(y,0,1) = \frac{e^{y}}{1+ e^{y}} \text{ Sigmoid function} $$
	This gives the link function (where q is the logit function and f is a Bernoulli output) $$ \frac{e^{\eta_{i}}}{1 + e^{\eta_{i}}} \leftrightarrow \eta_{i} = ln\left(\frac{µ_{i}}{1-µ_{i}}\right) $$
	The logit link is also called the canonical link since $$ \eta_{i} = ln\left(\frac{µ_{i}}{1-µ_{i}}\right) = \theta_{i} $$
	In general a canonical link si a g such that $\eta_{i}= g(µ_{i}) = \theta$ and it come naturally mapping the distribution to the EDF $µ_{i}= b'(\theta_{i}) \to g:(b')^{-1}$ (default option in glm)

4) Complementary Log-Log link $$ \eta_{i} = ln[-ln(1-µ_i)]$$

### Fitting a GLM
#### Likelihood
Since a choice of f and a choice of g, the parameters $\underline{\beta}$ are overestimated by maximum likelihood 
$$
\begin{align}
l(\underline{\beta}) &=  \sum_{}^{}log[f(y_{i}, \mu_{i}, \Phi)] \\
&\text{Where } f(y_{i}, \theta_{i},\Phi) = \exp \left\{ \frac{y_{i}\theta_{i} - b(\theta_{i})}{a_{i}(\Phi)} + c(y_{i}, \Phi) \right\} \text{ so the log can be written as} \\
l(\underline{\beta}) &= \sum_{}^{}\left\{ \frac{y_{i}\theta_{i} - b(\theta_{i})}{a_{i}(\Phi)} + c(y_{i}, \Phi) \right\} \\
\\
&\text{So that}: \\
&\begin{cases}
\theta_{i} \leftrightarrow µ_{i}\\
\eta{i} \leftrightarrow \underline{x}_{i}^{t}\underline{\beta}
\end{cases} \\
\\
&\text{So that the function can be mapped as}:\\
&µ_{i}= b'(\theta_{i}) \text{ and } g(µ_{i}) = \eta_{i}
\end{align}
$$

To obtain $\underline{\beta}$, we maximize the log-likelihood function. Since we cannot always find an analytical solution, we use numerical methods:
- [[Confidence Interval#Fisher Information|Fisher Scoring ]]
	- This is a variant of the Newton-Raphson method.
	- It is especially easy for Gaussian regression (where it has a closed form), but for other distributions it requires numerical iterations.
- Iteratively Reweighted Least Squares (IRWLS).
	- This method is mathematically equivalent to Fisher Scoring 
	- It is easier to implement in statistical software such as R because it reduces to an iterative weighted least squares problem, which is well known and stable.

##### Iteratively Reweighted Least Square
Suppose a Gaussian linear model $Y = X\beta + \epsilon$ and the condition where $\text{var}(Y) \propto f(\hat{\eta})$ and $\hat{y} = \hat{\eta} = X\hat{\beta}$. The variance of Y is proportional to a function of the estimated parameters because, in a GLM (as previously mentioned), the variance is linked to the mean, which in turn depends on the estimator. Therefore, the variance is not constant but is proportional to $\underline{\beta}$.  We would use weights $w_{i}$ where $w_{i}^{-1} = f(\hat{\eta})$ Since the weights are a function of an iterative $\hat{\beta}$ fitting procedure would be needed. We might set the weights all equal to one, estimate $\hat{\beta}$. use this to recompute the weights, reestimate $\hat{\beta}$, and so on until convergence.

We can use a similar idea to fit a GLM. we want to regress $g(y)$ on _X_ with weights inversely proportional to var $g(y)$. However, $g(y)$ might not make sense in some cases (for example, in the binomial GLM. So we linearize $g(y)$ as follows: Let $η=g(µ)$ and $µ=EY$. Now do a one-step expansion:

$$
\begin{align}
g(y) &\approx  g(µ) + (y - µ)g'(µ)\\
&= \eta + (y-µ)\frac{d\eta}{dµ} \\
&= z
\end{align}
$$
and 
$$
\widehat{var(z)} = \left(\frac{d\eta}{dµ}\right)^{2} \ \ V\left(\hat{µ} = \frac{1}{w}\right)
$$
So the IRWLS procedure would be:
1) Set the initial estimates $\hat{\eta}_{0}$ and $\hat{\mu}_{0}$
2) Form the “adjusted dependent variable” $z_{0} = \hat{\eta}_{0} + (y - \hat{\mu}_{0})\frac{d\eta}{dµ}|_{\hat{\eta}_{0}}$ 
3) Form the weights $w_{0}^{-1} = \left(\frac{d\eta}{dµ}\right)^{2}|_{\hat{\eta}_{0}}V(\hat{\mu_0})$
4) Reestimate $\beta$ to get $\hat{\eta}_{1}$
5) Iterate steps 2–3–4 until convergence.

Notice that the fitting procedure uses only $η=g(µ)$ and $V(µ)$, but requires no further knowledge of the distribution of _y_. 

Estimates of variance may be obtained from (W is the matrix of the weight): 
$$
\widehat{var}(\hat{\beta}) = (X^{T}WX)^{-1}\hat{\Phi}
$$
which is comparable to the form used in weighted least squares with the exception that the weights are now a function of the response for a GLM. 





