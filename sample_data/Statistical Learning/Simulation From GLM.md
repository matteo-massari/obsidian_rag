Ex 1 

Poisson Regression 

y response (count) $\underline{X} = x_{1},...,x_{p}$

Model Assumption 
$Y_{1} = y|\underline{X} = \underline{x}_{i}, \ \ i = 1,...,n$

$y_{1},...,y_{n}$ observation of $y_{i}$ that are independent but not identically distributed

$\mu_{i} = E[y_{i}] \in (0,\infty)$ mean parameter
$\eta_{i}= \underline{x}_{i}^{t}\underline{\beta}$
if we mapped to the GLM we get the canonical link function (g) as $log(\mu_{i}) = \eta_{i}$
$$y_{i} \sim \text{Poisson}(\mu_{i}) = \text{Poisson}(e ^{\eta_{i}}) = \text{Poisson}(e^{\underline{x}_{i}^{t}\underline{\beta}})$$
So if we want to explain in one line 
$$
Y_{i}= \text{Poisson}(e^{\underline{x}_{i}^{t}\underline{\beta}})
$$
**SImulation**
$$
\begin{align}
p = 1 , n = 100 
\end{align}
$$
1) For a simulation i have to generate 100 of value for $x_{1},...,x_{100}$ and 100 for Y
2) $\eta_{i} = \beta_{0}+ \beta_{1}x_{i}$. Fix $\beta_{0}= 1, \beta_{1} = 2$
3) Stochastic part: I sample $y_{i}$ from $\text{Poisson}(e ^{\eta_{i}}), \ \ i = 1,...,100$

![[Pasted image 20250321213537.png]]


![[Lecture 5.pdf]]