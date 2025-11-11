# Model selection for linear model 
Model selection (alternative to resampling methods) for parametric families ==we use criteria that make adjustment to the training error by accounting for model complexity.== 

The must common ones: 
1) AIC: Akaike information Criteria 
$$
AIC(M) = -2log(L) + 2d
$$
- log(L): log-likelihood on training data
- d: complexity of the model (number of parameter that define a model)

Consider different models $M_{i}$ and choose the model that minimises the AIC.

2) BIC: bayesian information criteria 
$$
BIC(M) = -2log(L) + log(n)d
$$
This can be applied for log(n) > 2, n > 7, so bigger weight on module complexity 

All this criteria allow us to get the optimum trade-off between bias and variance. 

## Trade - off between bias and variance
Remark
1) The true model may not be in the family of models that we consider 
2) Model selection depends on the data that we have
	example: The  generating model may have a polynomial of high degree but all see is a linear trend 
	This problem become more pronounced in high dimensional cases (p>>n), where even a simple linear model may not be possible 
3) The bias/variance trade-off is connected also with the bias and variance of $\underline{\hat{\beta}}$ 

**Linear model**
$Y$ response, $X_{1}, ..., X_{p}$ predictors. 

Consider a linear model
$$
Y = \beta_{0}+ \beta_{1}x_{1}+ ...+ \beta_{p}x_{p}+ \epsilon, \ \ \epsilon \sim N(0,\sigma^{2})
$$
Data: $(\underline{x}_{i}, y_{i}), \ \ i = 1,...,n$ 
Aim: Estimate $\underline{\beta}$ via least - square

**Least square**
In matrix form, the model can be written as:

$$
\begin{align}
\mathbf{Y} = \mathbf{X} \boldsymbol{\beta} + \boldsymbol{\varepsilon}
\end{align}
$$

Where:

- $\mathbf{Y} = \begin{pmatrix} y_1 \\ \vdots \\ y_n \end{pmatrix}$ dimension 1 x n
- $\mathbf{X} = \begin{pmatrix} x_{11} & \cdots & x_{1p} \\ \vdots & \ddots & \vdots \\ x_{n1} & \cdots & x_{np} \end{pmatrix}$  dimensione n x p 
- $\boldsymbol{\beta} = \begin{pmatrix} \beta_1 \\ \vdots \\ \beta_p \end{pmatrix}$  dimension p x 1
- $\boldsymbol{\varepsilon} = \begin{pmatrix} \varepsilon_1 \\ \vdots \\ \varepsilon_n \end{pmatrix}$

 Stima dei parametri con Minimi Quadrati (Least Squares)
$$
\begin{align}
\underline{\hat{\beta}} &= \arg\min_{\boldsymbol{\beta}} \sum_{i=1}^{n} \left( y_{i} - \mathbf{x}_{i}^{T} \boldsymbol{\beta} \right)^{2} \text{ Where } \mathbf{x}_i = \begin{pmatrix} x_{i1} \\ \vdots \\ x_{ip} \end{pmatrix} \\
&= \arg\min_{\boldsymbol{\beta}} \left( \mathbf{Y} - \mathbf{X} \boldsymbol{\beta} \right)^{T} \left( \mathbf{Y} - \mathbf{X} \boldsymbol{\beta} \right)\\
\end{align}
$$

$$
\begin{align}
S(\underline{\beta}) &= (\underline{y} - x \underline{\beta})^{T} ( \underline{y} - X\underline{\beta}) \\
&= \underline{y}^{T}\underline{y} - \underline{y}^{T}x\underline{\beta} - (x\underline{\beta})^{T}\underline{y} + (x\underline{\beta})^{t}x\underline{\beta} \\

\text{Remark } & (AB)^{t} = B^T A^T \\

&= \underline{y}^{t}\underline{y} - \underline{y}^{t}x\underline{\beta} - \underline{\beta}^{t}x^{t}\underline{y} + \underline{\beta}^{t}x^{t}x\underline{\beta} \text{ multiplied by them these became all scalar} \\
\text{Rewrite } &\underline{y}^{t}x\underline{\beta} = \underline{\beta}^{t}x^{t}\underline{y} \text{ so it become} -2\underline{\beta}^{t}x^{t}\underline{y} \\
&= \underline{y}^{t}\underline{y} - 2\underline{\beta}^{t}x^{t}\underline{y} + \underline{\beta}^{t}x^{t}x\underline{\beta} \\
\end{align}
$$
To minimize the $\beta$ we do the derivate 
$$
\begin{align}
\frac{\partial S(\underline{\beta})}{\partial \underline{\beta}} &=
-2x^{t}\underline{y} + 2x^{t}x\underline{\beta} \\
\end{align}
$$
And the set the derivate to 0
$$
\begin{align}
&-2x^{t}\underline{y} + 2x^{t}x\underline{\beta} = 0 \\
&(x^{t}x)\underline{\beta} = x^{t}\underline{y} \\
&\text{normal equation: p equation in p parameter}\\
&\text{It get a unique solution when } x^{t}x \text{ is inverted}\\
&\hat{\underline{\beta}} = (x^{t}x)^{-1}x^{t}\underline{y}
\end{align}
$$

**Bias / Variance of $\hat{\beta}$**
Under the assumption that data are generated from the linear model 
1) Expected value
$$
\begin{align}
E(\underline{\hat{\beta}})  &= E[(x^{t}x)^{-1}x^{t}\underline{Y}] \\
& = (x^{t}x)^{-1}x^{t} E[\underline{Y}] \\
\text{Reamrk } &E[y] = x\beta \\
&=  (x^{t}x)^{-1}x^{t}x\underline{\beta} \\
\text{Reamrk } &(x^{t}x)^{-1}x^{t}x = I \\
&= \underline{\beta} 
\end{align}
$$
This properties tell us that we have no bias in the media 

2) Variance (is a p x p matrix)
$$
\begin{pmatrix}
\operatorname{Var}(\hat{\beta}_1) & \operatorname{Cov}(\hat{\beta}_1, \hat{\beta}_2) & \cdots \\
\vdots & \ddots & \\
& & \operatorname{Var}(\hat{\beta}_p)
\end{pmatrix}$$
The calculation is
$$
\begin{align}
var[\underline{\hat{\beta}}] &= Var((x^{t}x)^{-1}x^{t}\underline{y})\\
\text{Semplify using  } &Var(A\underline{y}) = A \times Var(\underline{y}) \times A^{t} \\
&= (x^{t}x)^{-1}x^{t}\sigma^{2}x(x^{t}x)^{-1}\\
&= \sigma^{2}(x^{t}x)^{-1}(x^{t}x)(x^{t}x^{-1}) \\
&= \sigma^{2}(x^{t}x)^{-1}
\end{align}
$$
So the variance depends on the conformation of $(x^{t}x)^{-1}$ so the Gram matrix. Mathematically ... . In this context if i have multicollinearity between the predictor in $(x^{t}x)^{-1}$ then the $\beta_{j}$ will predict the same predictors, duplicating a trend in the training data and so augemntng the variance. 

So the variance of $\hat{\beta}$ depends on the structure of the matrix $(X^T X)^{-1}$, i.e., the inverse of the Gram matrix.  
Mathematically, if there is multicollinearity among the predictors, the Gram matrix becomes nearly singular, and the inverse (XTX)−1(X^T X)^{-1}(XTX)−1 becomes unstable.  

As a result, the estimated coefficients $\hat{\beta}_j$ become highly sensitive and may effectively model the same underlying trend, duplicating information from correlated predictors and thus increasing the variance.

**Bias / Variance of a linear model**
Assume that data are generated by a linear model :
$$
\begin{align}
E[(y_{0}- \hat{f}(\underline{x}_{0}))^{2}] = (f(\underline{x}_{0}) - E[\hat{f}(\underline{x}_{0})])^{2} + Var(\hat{f}(\underline{x}_{0})) + Var(\epsilon) \\
\end{align}
$$
we recompute bias and variance:
1) ==Bias==: $\underline{x_{0}}^{t}\underline{\hat{\beta}}$  
$$
E[\underline{x_{0}}^{t}\underline{\hat{\beta}}] = \underline{x_{0}}^{t}E[\underline{\hat{\beta}}] = \underline{x_{0}}^{t}\beta
$$

3) The bias depend on the behavior of $\hat{\beta}$ (is the same of $\beta$) (lezione 9 minuto 46)
4) ==Variance==
$$
\begin{align}
var[\underline{x_{0}}^{t}\underline{\hat{\beta}}] &= \sigma^{2} \underline{x}_{0}^{t}(x^{t}x)^{-1}\underline{x}_{0} \\
\text{This is just} & \text{ a number due to the different dimension of the element} \\
&= \sigma^{2}\text{Trace}((x^{t}x)^{-1}\underline{x}_{0}^{t}\underline{x}_{0}) \\
&= \sigma^{2}\text{Trace}\left(\left(\frac{x^{t}xn}{n}\right)^{-1}\underline{x}_{0}^{t}\underline{x}_{0}\right) \\
\frac{x^{t}x}{n} & \text{is the empirical covariance of the prediction} \\
\text{Assuming that } & E[x_{i}] = 0, \ Var(x_{i}) = 1 \text{ are independet}, \\
& = \sigma^{2} \text{Trace}((I_{p \times p}n)^{-1}\underline{x}_{0}^{t}\underline{x}_{0}) \\
&= \sigma^{2} \text{Trace}\left(\frac{1}{nI_{p}} \underline{x}_{0}^{t}\underline{x}_{0}\right)\\
& = \sigma^{2} \text{Trace}\left(\frac{1}{n}\underline{x}_{0}^{t}\underline{x}_{0}\right)\\
& \text{we do this approximation } \underline{x}_{0}^{t}\underline{x}_{0} \approx p,  \\
&\text{ and Since the result is scalar, the trace can be omitted} \\
& \approx \sigma^{2}\frac{p}{n}
\end{align}
$$

Remark **Trace of a matrix**
The trace(A) of a matrix is the sum of the diagonal element of that matrix. 
Properties of a trace:
- Trace(ABC) = trace(BCA) = Trace(CAB). i can di cyclic permutation of a matrix

Remark **empirical estimation of covariance**:
$Cov(W,Z) = E[WZ] - E[W]E[Z]$
$\widehat{Cov(W,Z)} = \sum_{i = 1}^{n} \frac{w_{i}z_{i}}{n}$ 


At the end, for a linear model,
$$
E[(y_{0}-\underline{x}_{0}^{t} \hat{\underline{\beta}})^{2}] \approx 0 + \sigma^{2} \frac{p}{n} + \sigma^{2} 
$$
SO prediction accuracy depends on $\frac{p}{n}$. In particular, we can distinguish 3 case:
1) n >> p
	- bias = 0
	- variance is small
	- Least Square will do very well in these case (if the model is well-specified)
2) $n \not\gg p$ (n not much larger than p like 10 observation what we want to fit a polynomial degree of 9
	- variability in $\hat{\underline{\beta}}$ and therefore in prediction 
	- overfitting, poor prediction 
3) n < p $x^{t}x$ is singular (not invertible), an we can have infinite solution, infinite variance (is like fit a line thought a point) 

Ls estimators of $\underline{\beta}$ has no bias but high variance when n≈p and infinite variance when n < p (high dimensional statistics). 
## Improving the prediction accuracy of linear model
Three main approach to improve prediction accuracy of linear model:
1) [[#Variable selection]] (Limit p):
	- Reduce variance, at the expense of some bias 
	- Interpretability; finding the most important predictions 
2) [[#Shrinkage methods]]
	Replace least square by alternative methods so that the variance of the estimators is smaller, at expense of some bias
3) Dimensionality reduction; Project some predictors into a lower dimensional space n (with n < p) and use n projection of predictors

### Variable selection
Identify a subset of most important predictions among $x_{1},...,x_{p}$. Ideally, one should consider all possible subset and evaluate the model and choose the best one

**Algorithm best subset**
1) Let $M_{0}$ is the null model $\beta_{0}$ 
2) For $k=1,...,p$:
	1) Fit all $\binom{p}{k}$ models with k predictors
	2) Pick the best among these fit (check RSS, MSE, ecc) and call it $M_{k}$
3) Select a single best model out of $M_{0}, M_{1},...,M_{p}$ using some model selection criteria (CV, AIC, BIC, ...) that account for model complexity. 

How many models?
p predictors we achieve $2^{p}$ possible model:
- $p = 2 \to [\text{only intercept}, x_{1}, x_{2}, x_{1}x_{2}] \to 2^{2}=4 \text{ models}$
- $p = 20 \to 2^{20}=1048576$

#### Stepwise methods 
**Forward**
Go to the simplest to the more complex one:
1) Let $M_{0}$ be the null model
2) For $k=0,1,...,p-1$
	1) Fit all p-k models that augment $M_{k}$ with one predictor
	2) Choose the best among them and call this $M_{k+1}$ via $R^{2}$
3) Select the best model among $M_{0},...,M_{p}$ according to some model selection criterion (CV, AIC, BIC,...)
We see this by fixing predictors to each step before adding new predictors 

i do only $\frac{p(1-p)}{p}$ model 
**La vinciotti li ha solo nominati**
#### Backward selection
Algoritmo Backward selection
1. Iniziamo 1. con tutte le variabili nel modello, modello saturo, $M_p$
2. Per k = 1, 2, · · · , p − 1:
    1. consideriamo tutti i p − k modelli che contengono tutti i predittori di $M_k$, eccetto uno, per un totale di k − 1 predittori.;
    2. scegliamo il migliore (minor RSS o maggiore $R^2$) tra questi k modelli e lo indichiamo con $M_{k-1}$.
3. Selezioniamo il miglior modello tra $M_0 ,..., M_p$ , utilizzando l’errore di previsione cross-validato, Cp, BIC o $R^2adj$.
Iniziamo rimuovendo dal modello saturo la variabile con il p-value più grande (cioè la variabile meno statisticamente significativa) e iteriamo la procedura. Una regola d'arresto definisce l'interruzione del ciclo (interrompere quando tutte le variabili rimanenti hanno un p-value al di sotto di una certa soglia).

Come per la [[#Forward selection]] a backward selection considera $1 + p(p + 1)/2$ modelli per applicare la selezione del miglior sottoinsieme e non garantisce la scelta del miglior modello contenente un sottoinsieme dei p predittori.
Ma contrariamente richiede n > p, quindi non può essere applicata per alta dimensionalità.

#### Mixed selection
Un ulteriore alternativa è una versione a versioni ibride della forward e backward selection:
1) Iniziamo senza variabili nel modello e, come con la forward selection, sequenzialemente aggiungiamo la variabile che fornisce il miglior adattamento.
2) eventualmente rimosse eventuali variabili che non forniscono più un miglioramento nell’adattamento del modello.
	I p-value possono diventare più grandi man mano che vengono aggiunti nuovi predittori al modello. Pertanto, se in un certo punto il p-value per una delle variabili nel modello sale al di sopra di una certa soglia, allora rimuoviamo quella variabile dal modello.
3) Si eseguono questi passaggi in avanti e indietro fino a quando tutte le variabili nel modello avranno un p-value sufficientemente basso, e tutte le variabili al di fuori del modello avrebbero un grande p-value se aggiunte al modello. 

Tale approccio cerca di imitare la selezione del miglior sottoinsieme mantenendo i vantaggi computazionali della selezione iterativa in avanti e all’indietro.

### Shrinkage methods
Least square estimators of $\underline{\beta}$ has no bias but high variance when n≈p and infinite variance when n < p (high dimensional statistics). Improve prediction accuracy of linear model by reducing p variable selection, which can be computationally expensive.

Alternative one can use other methods to estimate $\beta$. Some popular methods have emerged when $\beta$ is estimated by adding some constraint on the squared error objective function:
- [[#Ridge Regression]]
- [[#Lasso Regression]]

#### Ridge Regression
Also known in machine learning as [[Regression in Machine Learning#L2 Regularization|L2 regularization]]
For the least squared:
$$
\hat{\underline{\beta}}_{LS} = \arg\min_{\boldsymbol{\beta}} \left( \mathbf{Y} - \mathbf{X} \boldsymbol{\beta} \right)^{T} \left( \mathbf{Y} - \mathbf{X} \boldsymbol{\beta} \right)
$$
In the Ridge regression 
$$
\hat{\underline{\beta}}_{Ridge} = \arg\min_{\boldsymbol{\beta}} ( \underline{Y} - X \boldsymbol{\beta} )^{T} ( \underline{Y} - X \boldsymbol{\beta})
$$
But we apply a constraint has:
$$
\sum_{j = 1}^{p} \beta_{j}^{2} ≤ c \text{ for } c ≥ 0 \text{ (tuning parameter)}
$$
So if:
- $c = +\infty$ we obtain the least squared, because we don't apply any constraint
- c decrease, more weight is put on the constraint resulting in shrin
- ked estimation 

##### Estimation 
$$
\min_{\underline{\beta}} ( \underline{Y} - X \boldsymbol{\beta} )^{T} ( \underline{Y} - X \boldsymbol{\beta}) \text{ s.t } \sum_{j = 1}^{p} \beta_{j}^{2} ≤ c
$$
Constrained optimization (convex), it means that there is a global minimum at the problem. 

This problem can also been written with the langrangian function 
$$
\min_{\underline{\beta}} \max_{λ ≥ 0}[( \underline{Y} - X \beta )^{T} ( \underline{Y} - X\beta) + λ(\sum_{j = 1}^{p} \beta_{j}^{2} - c)]
$$
We solve a **min–max** problem where:
- the ==inner maximization over $\lambda \ge 0$ enforces the constraint by penalizing any violation==,
- the ==minimization over $\beta$ finds the best parameters== under that constraint.”

They can be defined identically because
$$
\begin{align}
\text{Consider }\underline{\beta} \text{ s.t } \sum_{j = 1}^{p} \beta_{j}^{2} > c &\Longleftrightarrow  \text{ (out the constraint)} \\
&\Longleftrightarrow \sum_{j = 1}^{p} \beta_{j}^{2} - c > 0 \text{ this term is positive}\\
&\Longleftrightarrow \max_{\lambda ≥ 0}() = +\infty, \text{so } \beta \text{ is not the minimum} \\
\text{Consider }\underline{\beta} \text{ s.t } \sum_{j = 1}^{p} \beta_{j}^{2} ≤ c &\Longleftrightarrow  \text{ (inside the constraint)} \\ 
&\Longleftrightarrow \beta_{j}^{2} - c < 0 \text{ this term is negative, remove thing} \\
&\Longleftrightarrow \lambda = 0 \text{ and find the minimum }  (\underline{Y} - X \beta )^{T} ( \underline{Y} - X\beta)
\end{align}
$$

Also in good to realize that this quantity 
$$
 λ\left(\sum_{j = 1}^{p} \beta_{j}^{2} - c\right)=  λ\sum_{j = 1}^{p} \beta_{j}^{2} - λc
$$
So λ depend also from c, that is an hyperparameter. Also i can look at λ has my tuning parameter

The original problem is equivalent to:
$$
\min_{\underline{\beta}}( \underline{Y} - X \beta )^{T} ( \underline{Y} - X\beta) + λ\sum_{j = 1}^{p} \beta_{j}^{2} \text{ with } λ \text{ as tunign parameter}
$$
Meaning that i take only λ has hyperparameter because if it depend also from c:
- $\lambda = 0$ we have the LS:
- $\lambda > 0$ puts more weight on the penalty

**Minimization**
To minimize $\min_{\underline{\beta}}( \underline{Y} - X \beta )^{T} ( \underline{Y} - X\beta) + λ\sum_{j = 1}^{p} \beta_{j}^{2}$ we compute the first derivate
$$
\begin{align}
\frac{d}{d\underline{\beta}}\left[( \underline{Y} - X \beta )^{T} ( \underline{Y} - X\beta) + λ\sum_{j = 1}^{p} \beta_{j}^{2}\right] 
= -2x^{t}\underline{y} + 2x^{t}x\underline{\beta} + 2λ\underline{\beta}
\end{align}
$$
Then we find the $\beta$ by doing $-2x^{t}\underline{y} + 2x^{t}x\underline{\beta} + 2λ\underline{\beta} = 0$
$$
\begin{align}
-2x^{t}\underline{y} + 2x^{t}x\underline{\beta} + 2λ\underline{\beta} &= 0 \\
-x^{t}\underline{y} + x^{t}x\underline{\beta} + λ\underline{\beta} &= 0
\\
x^{t}\underline{y} &= x^{t}x\underline{\beta} + λ\underline{\beta} \\
x^{t}\underline{y} &= (x^{t}x + λI)\underline{\beta} \\
\hat{\underline{\beta}}_{Ridge} &= (x^{t}x + λI)^{-1} (x^{t}\underline{y})
\end{align}
$$
Analise of the $λI$:
1) is the only difference with the LS
2) is a scalar multiplied by an identity matrix 


$$ \begin{bmatrix}
\lambda & 0 & 0 & \ldots & 0 \\
0 & \lambda & 0 & \ldots & 0 \\
0 & 0 & \lambda & \ldots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \ldots & \lambda
\end{bmatrix} $$
The entire minimization in 
1) Closed - form expression, so we don't lose any information as LS
2) It depends on λ 
3) Computationally very efficient
4) $\text{For } \lambda > 0, \quad X^T X + \lambda I \text{ is always invertible,} \text{so } \hat{\beta}_{\text{ridge}} \text{ exists also when } n < p$

*Path of solution*
![[path_of_solution.png|300]]
λ has a role in the bias/variance trade-off, so it basically selected by cross-validation. This is the usual output for ridge regression. The red line is the one that correspond to the optimum λ obtained via CV. But this not satisfy the fact that the lines become smaller but it will not exactly be 0

##### Bias and Variance trade-off ridge 
We starts from a baseline of the 
$$
\begin{align}
\hat{\underline{\beta}}_{Ridge} =& (x^{t}x + λI)^{-1} (x^{t}\underline{y}) \\
\text{To make it} & \text{ more similar to the LS } R=x^{t}x \\
=& (R + λI)^{-1} x^{t}y \\
\text{Add } & \ \ I = RR^{-1}\\
=& (R + λI)^{-1} RR^{-1}x^{t}y \\
\text{Substitute } & \ \ R^{-1}=(x^{t}x)^{-1} \\
=& (R + λI)^{-1} R(x^{t}x)^{-1}x^{t}y \\
\text{This result in } &  \hat{\beta}_{LS} = (x^{t}x)^{-1}x^{t}y \\
=& (R + λI)^{-1} R\hat{\beta}_{LS} \\
\text{Gather R} \\
=& (R(I + λR^{-1}))^{-1}R\hat{\beta}_{LS} \\
\text{Take Out} & \ R \text{ from the }()^{-1} \text{ using }(AB)^{-1} = B^{-1}A^{-1} \text{propertie}\\
=& (I + λR^{-1})^{-1}R^{-1}R\hat{\beta}_{LS} \\
\text{Subsitutute } & R^{-1}R = I \\
=& (I + λR^{-1})^{-1}I\hat{\beta}_{LS} \\
\text{So the } & \text{ if we confront} \\
\hat{\underline{\beta}}_{Ridge} =& A\hat{\underline{\beta}}_{LS}
\end{align}
$$

In terms of **Bias**:
$$\begin{align}
E[\hat{\underline{\beta}}_{Ridge}] &= (I + λR^{-1})^{-1}E[\hat{\beta}_{LS}]\\
\text{Substitue } & E[\hat{\beta}_{LS}] = \underline{\beta}\\
&= (I + λR^{-1})^{-1}\underline{\beta}
\end{align}
$$
We don't have any bias if the λ is = 0. So for this method the trade-off is to sacrifies some bias, to obtain a reduction on Variance. So increasing λ increases the bias.

In term of **Variance**:
$$
\begin{align}
Var[\hat{\underline{\beta}}_{Ridge}] &= Var((I + λR^{-1})^{-1}\hat{\beta}_{LS})\\
\text{Propertie of the Var} \\ Var(AZ) &= A \times Var(Z) \times A^{t}\\
&= (I+\lambda R^{-1})^{-1} \sigma^{2}(x^{t}x)^{-1}(I + \lambda R^{-1})^{-1}
\end{align}
$$
If we plot as in function of λ 
We want to find the best trade-off from variance and bias. 
![[Screenshot 2025-04-08 at 12.30.17.png]]

##### Limitation of ridge regression
It dosen't really allow us to know what are the best predictors. (so we use lasso)

#### Lasso Regression
Also known in machine learning as [[Regression in Machine Learning#L1 Regularization|L1 regularization]]

Least Absolute shrinkage and selection operator (LASSO), has a different penalty to Ridge, in fact it has the ABSOLUTE value that give the variable SELECTION. 

$$
\hat{\underline{\beta}}_{lasso}  = \arg\min_{\beta} ( \underline{Y} - X \boldsymbol{\beta} )^{T} ( \underline{Y} - X \beta) + λ \sum_{j = 1}^{p}|\beta_{j}| \text{ with } λ ≥ 0
$$
As before this is equivalent to: 
$$
\min_{\beta} ( \underline{Y} - X \boldsymbol{\beta} )^{T} ( \underline{Y} - X \beta) \text{ s.t. }\sum_{j = 1}^{p}|\beta_{j}| ≤ c
$$
The tuning parameter is c:

Role of λ:
- λ = 0, we obtain the least square
- As λ increase (or decrease) more weight is put on the penalty
- One $\hat{\underline{\beta}}_{lasso}$ for each λ, but no closed-form expression
- Efficient numerical algorithm for finding $\hat{\underline{\beta}}_{lasso}$
- λ is related to bias/variance trade-off and must be selected by model selection criteria like cross - validation
- For λ sufficiently large, the $\hat{\underline{\beta}}_{lasso}$ estimate and shrunk exactly to zero, like variable selection.



#### Lasso vs Ridge
Why lasso estimate can be exactly zero?

Let us consider just two predictors, so  two parameters ($p_{1}$ and $p_{2}$). The objective function is $(\underline{y} - x\underline{\beta})^{t}(\underline{y} - x\underline{\beta}) = \sum_{i =1}^{n}(y_{i}-\underline{x}_{i}^{t}\underline{\beta})^{2}$. 

Remark $RSS = (\underline{y} - x\underline{\beta})^{t}(\underline{y} - x\underline{\beta})$ 

![[Screenshot 2025-04-08 at 13.43.05.png|300]]

To have a comparison between Lasso and Ridge, we can evaluate the constrains 

**Lasso**
$$
\sum_{j = 1}^{p}|\beta_{j}| ≤ c \text{ equal to say } |\beta_{1}| + |\beta_{2}| ≤ c
$$
When c is sufficiently small meaning that the least square is not in the diamond, one of the circle will hit it 
![[lasso_constraint.png|300]]

**Ridge regression**
$$
\sum_{j = 1}^{p}\beta_{j}^{2} ≤ c
$$
![[ridge_constraint.png|300]]

**Example**
Let's assume that we have only one predictor (p=1) (lasso consider 1 predicctor at the time by default )
$$
\hat{\underline{\beta}}_{lasso}  = \arg\min_{\beta} ( \underline{Y} - X \boldsymbol{\beta} )^{T} ( \underline{Y} - X \beta) + λ |\beta_{j}| \text{ with } λ ≥ 0
$$
In this case i don't have the summatory, because i'm doing the lasso for one parameter. 

Derivate
$$
\begin{align}
-2x^{t}(\underline{y} -\underline{x}\beta) + λ\text{sign}(\beta)\\
\text{with }\text{sign}(\beta) = 
\begin{cases} 
1 \text{ if } \beta > 0  \\
-1 \text{ if } \beta < 0 \\
[-1,1] \text{ if } \beta = 0 \end{cases}

\end{align}
$$
![[Screenshot 2025-04-08 at 14.00.29.png|300]]
Is seem that can create problem, but is the corner that give us the effective variable selection.
In this case more precisely we are asking for 0. So we want to find an interval. Because the interval give me the range of value where my $\hat{\beta}$ is 0

#### Lasso vs least square
This parts solves also the Lasso regression minimization problem
$\hat{\underline{\beta}}_{lasso}$ versus $\hat{\underline{\beta}}_{LS}$

For $\beta>0$ 
$$
\begin{align}
\hat{\underline{\beta}}_{lasso} > 0 
&\Rightarrow x^{t}(y-x\hat{\beta}_{lasso}) = \frac{λ}{2}\\
&\Rightarrow x^{t}y = x^{t}x\hat{\beta}_{lasso} + \frac{λ}{2} \\
&\Rightarrow \hat{\beta}_{lasso} = (x^{t}x)^{-1}x^{t}\underline{y} - (x^{t}x)^{-1}\frac{λ}{2} \\
&\Rightarrow \hat{\beta}_{lasso} =  \hat{\beta}_{LS} - (x^{t}x)^{-1}\frac{λ}{2}
\end{align}
$$
For $\beta < 0$ 
$$
\begin{align}
\hat{\underline{\beta}}_{lasso} < 0 
&\Rightarrow x^{t}(y-x\hat{\beta}_{lasso}) =  -\frac{λ}{2} \\
&\Rightarrow \beta_{lasso} =  \hat{\beta}_{LS} + (x^{t}x)^{-1}\frac{λ}{2}
\end{align}
$$
For $\beta = 0$
$$
\begin{align}
\hat{\underline{\beta}}_{lasso} = 0 
&\Rightarrow x^{t}(y-x\hat{\beta}_{lasso})  \in \left[-\frac{λ}{2},\frac{λ}{2}\right]\\
&\Rightarrow (x^{t}x)^{-1}x^{t}y-(x^{t}x)^{-1}\hat{\beta_{l}} \in \left[-(x^{t}x)^{-1}\frac{λ}{2},(x^{t}x)^{-1}\frac{λ}{2}\right]\\
&\Rightarrow \hat{\underline{\beta}}_{ls}  \in \in \left[-(x^{t}x)^{-1}\frac{λ}{2},(x^{t}x)^{-1}\frac{λ}{2}\right]
\end{align}
$$
	So we have found a sort of threshold where if the $\hat{\underline{\beta}}_{ls}$ falls in this area the $\hat{\beta}_{l}$ is = 0. For a fixed λ is $-(x^{t}x)^{-1}\frac{λ}{2},(x^{t}x)^{-1}\frac{λ}{2}$

We can plot this relationship via the soft - thresholding operator
![[Screenshot 2025-04-08 at 14.50.44.png]]


- $\lambda$ is related to the **bias/variance trade-off**
- $\lambda$ induces **sparsity** (variable selection)

The **soft-thresholding operator** is used as the basis of a **coordinate descent algorithm** for the estimation of $\beta$ via Lasso
$$\hat{\beta}_{\text{lasso}} \in \left[-(X^T X)^{-1} \frac{\lambda}{2},\ (X^T X)^{-1} \frac{\lambda}{2}\right]$$



#### Final Comments
1. Both **ridge** and **lasso** can be computed even when $n < p$
2. They **reduce variance** at the expense of **bias**
3. $\lambda$ must be **carefully chosen**
4. They have been **extended in many directions**:
    - penalties  
    - generalized linear models (GLMs)
