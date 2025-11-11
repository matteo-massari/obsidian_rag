# Support Vector Machine
Support vector machine are a supervised, non-parametric classification method.

## Assumption
**Binary classification** (2D, 2 classes)
For the [[Statistical decision theory#Bayes classifier|bayes classifier]] $p(1|\underline{x})$ we use the 0-1 loss with a 0.5 threshold, like a cut off to a probability $p(1|\underline{x}) > 0.5$ then classify to class 1
![[Screenshot 2025-04-29 at 09.36.09.png|200]]

All the method so far (logistic regression, Linear discriminant analysis, knn and decision tree), find approximation of $p(1|\underline{x})$ 

Example
$\hat{p}(1|\underline{x}) = \frac{e^{\hat{\beta}^{t}\underline{x}}}{1+ e^{\hat{\beta}^{t}\underline{x}}}$ then $\hat{p}(1|\underline{x}) = 0.5 \Leftrightarrow \hat{\beta}^{t}\underline{x} = 0$ (decision surface) 

So if $\hat{\beta}^{t}\underline{x} = 0$ is the decision surface:
$$
\begin{cases}
\hat{\beta}^{t}\underline{x} >> 0 \to \hat{p}(1|\underline{x}) \approx 1\\
\hat{\beta}^{t}\underline{x} << 0 \to \hat{p}(1|\underline{x}) \approx 0
\end{cases}
$$

![[Screenshot 2025-04-29 at 09.47.41.png|300]]

==Support vector machine concentrate their effort on finding a function $g(\underline{x})$ that best separate the two classes ("local")==

We can perform classification via [[#hyperplane]]: 
### Hyperplane
Def: In a p-dimensional space, a hyperplane is a flat affine subspace of dimension p-1 :
- $p = 2 \to \text{line} \to \beta_{0}+ \beta_{1}x_{1} + \beta_{2}x_{2} = 0$
- $p = 3 \to \text{plane} \to \beta_{0}+ \beta_{1}x_{1} + \beta_{2}x_{2} + + \beta_{3}x_{3} = 0$
- $p = n \to \text{hyperplane} \to \beta_{0}+ \beta_{1}x_{1} + \beta_{2}x_{2} + ...+ \beta_{p}x_{p} = 0 \Leftrightarrow \beta_{0} + \underline{\beta}^{t}\underline{x} = 0 \text{ with } \underline{\beta} = \begin{pmatrix}\beta_{1} \\ \vdots \\ \beta_{p}\end{pmatrix}$

So one hyperplane $\to$ two halves 
![[Hyperplanes.png|300]]
### Classification Using a Separating Hyperplane
Data: $X_{n \times p}, \underline{x}_{1}, ..., \underline{x}_{n}$
Response: Observation falls into two classes, denoted with -1 and 1 $y_{1},..., y_{n} \in \{-1,1\}$
Assume that observation can be separated by a hyperplane. Then 
$\beta_{0} + \beta_{1}x_{i1} + ... +  \beta_{1}x_{ip} > 0 \ \text{if} \ y_{i}=1$ 
$\beta_{0} + \beta_{1}x_{i1} + ... +  \beta_{1}x_{ip} < 0 \ \text{if} \ y_{i}=-1$ 

Equivalencing a separating hyperplane satisfies:
$$
y_{i}(\beta_{0} + \beta_{1}x_{i1} + ... \beta_{p}x_{ip}) > 0 \ \ \forall i = 1,...,n
$$
![[AB940178-4B36-4A4A-B219-A6C277A1685F_1_102_a.jpeg]]

Based on this assumption an hyperplane can be used for classification
A test observation is assigned a class depending on which side of the hyperplane it is located so we classify the test observation $\underline{x}^{*}$ based on the sign of $f(x^{*})$ so that 
$$
f(x^{*}) = \beta_{0}+ \beta_{1}x_{1}^{*} + ... +\beta_{p}x_{p}^{*} \begin{cases} > 0 \to y^{*} = 1 \\ < 0 \to y^{*} = -1 \end{cases}
$$

We can also make use of the magnitude of $f(x^{*})$. If $f(x^{*})$ is far from zero, then this means that $x^{*}$ lies far from the hyperplane, and so we can be confident about our class assignment for $x^{*}$. On the other hand, if $f(x^{*})$ is close to zero, then $x^{*}$ is located near the hyperplane, and so we are less certain about the class assignment for $x^{*}$.

**Example**

| $y$ | $f(x)$ | $y \cdot f(x)$ | Interpretazione                      |
| --- | ------ | -------------- | ------------------------------------ |
| +1  | +3     | 3              | Corretta, confidenza alta            |
| -1  | -2     | 2              | Corretta, confidenza media           |
| +1  | -1     | -1             | Errore: classificato -1 invece di +1 |
| -1  | +0.5   | -0.5           | Errore, ma vicino al margine         |

## Type of support vector machine
- [[#Maximal margin classifier]] (hard margin classifier)
	It needs linearly separable classes
- [[#Support vector classifier]] (soft margin classifier)
	It is the case for a linear decision surface, for the case of a non separable classes 
- [[#Support vector machine]] (Kernel methods)
	extend to non-linear classifier

### Maximal margin classifier
Given linear separable classes, find the best separating [[#hyperplane]]
![[Screenshot 2025-04-29 at 09.59.36.png]]

Optimal separating hyperplane: The one that maximizes the [[Università/Magistrale/Introduction to Machine Learning/Support vector machine#The margin|margins]]. 
![[maximal_margin_classifier.png|300]]
**Margins**
The margins are the distance of a point (the support vector) to an hyperplane:
A hyperplane is defined by its orthogonal direction $\underline{\beta}$. Let us consider a $\underline{\beta}$ such that the norm is  $$||\underline{\beta}|| = \sqrt[]{\beta_{1}^{2} + ... + \beta_{p}^{2}}= 1$$


|                                                                                                                                       |                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| - $z$: point on the hyperplane<br>- $x_{t}$ generic point on the dataset<br>- $\underline{\beta}^t$ vector perpendicular to the plane | ![[Screenshot 2025-04-29 at 10.44.59.png\|200]] |


$$
\begin{align}
\text{dist}(\underline{x}, H) &= \frac{|\underline{\beta}^{t}(\underline{x}-\underline{z})|}{||\underline{\beta}||} \text{ note: }||\underline{\beta}|| \text{ is = 1} \\
&= |\underline{\beta}^{t}\underline{x}-\underline{\beta}^{t}\underline{z}| \\
&
\text{z is a generic point on the plane in particular }  \beta_{0} + \underline{\beta}^{t}\underline{z} =0 \\
&= |\underline{\beta}^{t}\underline{x}-\beta_{0}| \text{ the magintude of this, whichn is the distance of the point to} \\
&\text{the hyperplane is used to measure out the confidence of the prevision}
\end{align}
$$

**Find the hyperplane**
1) For each training point, the function computes its **signed distance** from the hyperplane, and then takes the **minimum** over all points.  
$$
\text{Signed distance} = y_i (\beta_0 + \boldsymbol{\beta}^T x_i)
$$
2) We want to take the point closest to the hyperplane, that is, the one with the smallest margin $$\min_{i=1,\ldots,n} y_i (\beta_0 + \boldsymbol{\beta}^T x_i)$$
	This will define the margins
3) The goal is to **maximize this margin**: that is, to maximize the minimal distance between the hyperplane and the training observations. The maximal margin hyperplane is defined as:
$$
(\hat{\beta_0}, \hat{\boldsymbol{\beta}}) = \arg\max_{\beta_0, \boldsymbol{\beta} \ \text{subject to} \ ||\boldsymbol{\beta}|| = 1} \left( \min_{i=1,\ldots,n} y_i (\beta_0 + \boldsymbol{\beta}^T \boldsymbol{x}_i) \right)
$$

where $|\beta_0 + \boldsymbol{\beta}^T \boldsymbol{x}_i|$ represents the distance from the point $x_i$ to the hyperplane defined by $\boldsymbol{\beta}$.


This function measures the **margin associated with a given hyperplane**.  
Clearly, the margin depends on the choice of $\boldsymbol{\beta}$.

So Equivalently:
$$
\max_{\beta_{0},...,\beta_{p}} M \text{ such that } \beta^{2}_{1} + \beta^{2}_{2} + ... + \beta^{2}_{p} = 1, y_{i}(\beta_{0}+ \underline{\beta}^{t}\underline{x}_{i}) ≥ M , \ i = 1,...,n
$$

Some Remarks:
1) there are at least two observation that are closest to the hyperplane and on the other side of the hyperplane. If there are only 2, the hyperplane is the one that is orthogonal to the blue dot and the red dot, 
2) In general, there are not many observation that are closest to the hyperplane, the one that are are called support vectors, and completely characterize the hyperplane

Two issues:
1) Classes are typically not linearly separable 
2) The classifier is sensitive to a small number of support vectors (risk of overfitting, not robust to outliers ...) 

### Support vector classifier 
The main solution is to relax the condition of separable class, by allowing some violation going from an hard margin to a soft margin resulting in support vector classifier 
The optimization is:
$$
\max_{\begin{align}\beta_{0},..., \beta_{p} \\ \epsilon_{1},...,\epsilon_{n}\end{align}} M \text{ such that } \beta^{2}_{1} + ... + \beta^{2}_{p} = 1, y_{i}(\beta_{0}+ \underline{\beta}^{t}\underline{x}_{i}) ≥ M(1-\epsilon_{i}) , \ i = 1,...,n
$$
where $\epsilon$ are **slack variable**.

#### Slack variable
Slack variable are variable that is added to an inequality constraint to transform it into an equality constraint. 

So to use slack variable to relax the margin M with $M(1-\epsilon_{i})$ for $\epsilon_{i} ≥ 0$ i need a number of maximal violation that i might have an that is $\sum_{i = 1}^{n} \epsilon ≤ C$ for $C ≥ 0$. This i constraint optimization problem. C is called slack budget and is a tuning parameter. It measure out the number of violation that i can accept so the idea is to fit C (with cross validation) and fine $\hat{\beta}_{0}, \underline{\hat{\beta}}, \hat{\underline{\epsilon}}$ that solves the constrained optimization problem. Then use $\hat{\beta}_{0}, \underline{\hat{\beta}}$ for classification (because the design an hyperplane). 
![[Slack_variables.png|300]]
Foto 

- $\epsilon_{i} = 0$ for observation on the correct side of margin 
- $\epsilon_{i} > 0$ 
	- $0 < \epsilon_{i}<1$ then $1-\epsilon_{i}> 0$ observation on the right side of the hyperplane 
- $\epsilon_{i} > 1$ are misclassified observation 

**Choice of C**
The choice of the number of violation
$$
\sum_{i = 1}^{n} \epsilon_{i} ≤ C 
$$
In particular if i choose C
- $C = 0 \implies \epsilon_{1} = ... = \epsilon_{n} = 0$ so i get the [[#maximal margin classifier]] 
- $C > 0 \implies$ The choice of the upper bound of the number of violation
So c is connected with bias/variance trade-off, In particular
1) C small 
	- $\implies$ hyperplane with a small margin and very few violation
	- $\implies$ a small number of support vectors (point on the margin + violations)
	- $\implies$ low bias but high variance
2) C large 
	- $\implies$ Hyperplane with large margin have many violation 
	- $\implies$ high bias but low variance

So use cross-validation for the selection of C


#### Alternative formulation of the optimization problem 
$$
\max_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} M  
$$
such that the constraint are:
- $C_{1} \ \ ||\underline{\beta}||^{2} = 1$ that is saying $\beta^{2}_{1} + ...+ \beta^{2}_{p} = 1$ 
- $C_{2} \ \  y_{i}(\beta_{0}+ \underline{\beta}^{t}\underline{x}_{i}) ≥ M(1-\epsilon_{i}), i = 1,...,n$ 
- $C_{3} \ \ \epsilon_{i} ≥ 0, i=1,...,n$ because under 0 it doesn't make sense
- $C_{4} \sum_{i = 1}^{n} \epsilon_{i} ≤ C$ where C is fixed


In optimization prospective there are preferred a ==convex function so that it have a global maximum of a global minimum==. The first constrain geometrically speaking is a circle which is not convex. 
So i want to get rid of $C_{1}$ by replace $C_{2}$ with a normalized vector $\tilde{C_{2}}: \frac{1}{||\beta||} y_{i}(\beta_{0}+ \underline{\beta}^{t}\underline{x}_{i}) ≥ M (1-\epsilon_{1})$ with $\frac{\underline{\beta}}{||\underline{\beta}||}$ the orthogonal vector of length 1. 
So now the 
$$\tilde{C_{2}} \Leftrightarrow y_{i}(\beta_{0}+ \underline{\beta}^{t}\underline{x}_{i}) ≥ ||\beta|| M (1-\epsilon_{i})$$
We now consider the $\beta$ such that $||\underline{\beta}|| = \frac{1}{M}, \ \ i,e. \ \ M = \frac{1}{||\underline{\beta}||}$ so that the optimization problem become:
$$
\max_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} \frac{1}{||\underline{\beta}||}
$$
sucht that 
- $C_{2} \ \  y_{i}(\beta_{0}+ \underline{\beta}^{t}\underline{x}_{i}) ≥(1-\epsilon_{i}), i = 1,...,n$ 
- $C_{3} \ \ \epsilon_{i} ≥ 0, i=1,...,n$
- $C_{4} \sum_{i = 1}^{n} \epsilon_{i} ≤ C$ where C is fixed

To avoid the fraction the whole problem can be written as a ==quadratic constrained optimization== (so with a global minimum) 
$$
\min_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} ||\underline{\beta}||^{2}
$$
such that:
- $C_{1}: y_{i}(\beta_{0} + \beta^{*}\underline{x}_{i})  ≥ 1-\epsilon_{i} \,\ i=\,...,n$ 
- $C_{2}: \epsilon_{i} ≥ 0, \ i = 1,...,n$
- $C_{3}: \sum_{i = 1}^{n} \epsilon_{1}≤C$

![[Screenshot 2025-04-29 at 15.25.27.png|300]]
The conditions can also been written as:
- $C_{1}: -[y_{i}(\beta_{0} + \beta^{*}\underline{x}_{i})-1+\epsilon_{i}] ≤ 0 \,\ i=\,...,n$ 
- $C_{2}: -\epsilon_{i} ≤ 0, \ i = 1,...,n$
- $C_{3}: \sum_{i = 1}^{n} \epsilon_{1} - C ≤ 0$

**Optimization process**
1) We ==define the generalized Lagrangian== for SVM as:

$$
\mathcal{L}(\beta_0, \beta, \xi, \gamma, \delta, \mu) =
\frac{1}{2} \|\beta\|^2
- \sum_{i=1}^n \gamma_i \left[ y_i(\beta_0 + \beta^\top x_i) - 1 + \xi_i \right]
- \sum_{i=1}^n \delta_i \xi_i
+ \mu \left( \sum_{i=1}^n \xi_i - C \right)
$$
Where:
- $\alpha = (\beta_0, \beta, \xi)$ are the primal variables
- $\gamma_i, \delta_i, \mu \geq 0$ are the **Lagrange multipliers**

Then the original constrained problem:
$$
\min_{\beta_0, \beta, \xi} \frac{1}{2} \|\beta\|^2 \quad \text{subject to constraints}
$$
We are working in a string duality setting so 
is equivalent to:

$$
\min_{\beta_0, \beta, \xi} \max_{\gamma, \delta, \mu \geq 0} \mathcal{L}(\cdot) \quad \text{(Primal)} = \max_{\gamma, \delta, \mu \geq 0} \min_{\beta_0, \beta, \xi} \mathcal{L}(\cdot) \quad \text{(Dual)}
$$

This is the standard min-max to max-min conversion valid under convexity assumptions.
$$
\mathcal{L}(\cdot) = \frac{1}{2} \|\beta\|^2 - \sum_{i=1}^n \gamma_i \left[ y_i(\beta_0 + \beta^\top x_i) - 1 + \xi_i \right]
- \sum_{i=1}^n \delta_i \xi_i + \mu \left( \sum_{i=1}^n \xi_i - C \right)
$$

We use the Karush–Kuhn–Tucker condition to perform the optimization process
2) Then ==we compute the derivate for the lagrange optimization== (*stationary*):
**W.r.t.** $\beta_0$:
$$
\frac{\partial \mathcal{L}}{\partial \beta_0} = -\sum_{i=1}^n \gamma_i y_i = 0 \quad \Rightarrow \quad \sum_{i=1}^n \gamma_i y_i = 0
$$
**W.r.t.** $\beta$:
$$
\frac{\partial \mathcal{L}}{\partial \beta} = \beta - \sum_{i=1}^n \gamma_i y_i x_i = 0 \quad \Rightarrow \quad \beta = \sum_{i=1}^n \gamma_i y_i x_i
$$
**W.r.t.** $\xi_i$:
$$
\frac{\partial \mathcal{L}}{\partial \xi_i} = -\gamma_i - \delta_i + \mu = 0 \quad \Rightarrow \quad \gamma_i = \mu - \delta_i
$$

Since $\delta_i \geq 0$, it follows that:
$$
\gamma_i \leq \mu \quad \forall i
$$
*Plug Back into the Lagrangian (Dual Form)*
3) Now substitute the expressions into the Lagrangian to get the dual function:
Extend the Lagrangian:
$$
\mathcal{L} =
\frac{1}{2} \beta^\top \beta
- \sum_{i=1}^n \gamma_i y_i \beta^\top x_i
- \sum_{i=1}^n \gamma_i y_i \beta_0
+ \sum_{i=1}^n \gamma_i
- \sum_{i=1}^n \gamma_i \xi_i
- \sum_{i=1}^n \delta_i \xi_i
+ \mu \sum_{i=1}^n \xi_i
- \mu C
$$
And plugin the derivative as:
$$
\theta_D(\gamma, \delta, \mu) = \frac{1}{2} \beta^\top \beta - \sum_{i=1}^n \left( \gamma_i y_i \beta_0 + \gamma_i y_i \beta^\top x_i - \gamma_i + \gamma_i \xi_i \right)
- \sum_{i=1}^n \delta_i \xi_i + \mu \sum_{i=1}^n \xi_i - \mu C
$$
Eliminating all terms and simplifying:
$$
= \sum_{i=1}^n \gamma_i - \frac{1}{2} \sum_{i,j=1}^n \gamma_i \gamma_j y_i y_j x_i^\top x_j - \mu C
$$
The slack terms vanish because:
$$
\sum_{i=1}^n \xi_i (\mu - \gamma_i - \delta_i) = 0
$$
(due to the stationarity condition above).

The we transform the primal problem in the **dual problem**, the original constrained problem is equivalent to 
$$
\max_{\gamma, \mu} \sum_{i=1}^n \gamma_i - \frac{1}{2} \sum_{i,j=1}^n \gamma_i \gamma_j y_i y_j \langle x_i, x_j \rangle - \mu C
$$

Subject to:
- $0 \leq \gamma_i \leq \mu$
- $\sum_{i=1}^n \gamma_i y_i = 0$

the Equivalent Standard Form
$$
\max_{\gamma} \sum_{i=1}^n \gamma_i - \frac{1}{2} \sum_{i,j=1}^n \gamma_i \gamma_j y_i y_j \langle x_i, x_j \rangle
$$
Subject to:
- $0 \leq \gamma_i \leq \mu$
- $\sum_{i=1}^n \gamma_i y_i = 0$

 Notes
- This dual formulation is entirely written in terms of **inner products** $\langle x_i, x_j \rangle$
	This enables **kernel methods**, where inner products can be replaced by kernel functions $K(x_i, x_j)$
- $\mu$ is the **new tuning parameter** in the dual
	It has an **inverse relationship** with $C$ in the primal: $$C = 0 \iff \mu = +\infty$$
	So $\mu$ plays the **opposite role** of $C$.

At the optimum, we have:
$$
\hat{\beta} = \sum_{i=1}^n \hat{\gamma}_i y_i x_i
$$
- $\hat{\beta}$ is $p$-dimensional (feature space)
- $\hat{\gamma}$ is $n$-dimensional (sample size)

Sparsity of $\hat{\gamma}_i$ , where any of the $\hat{\gamma}_i$ are **zero**.
Using the **KKT conditions** (complementary slackness):

$$
\hat{\gamma}_i \cdot g_i(\hat{\alpha}) = 0
$$
Which becomes:
$$
\hat{\gamma}_i \left[ y_i (\hat{\beta}_0 + \hat{\beta}^\top x_i) - 1 + \hat{\xi}_i \right] = 0, \quad i = 1, \dots, n
$$

If a point is on the correct side of the hyperplane:
$$
y_i (\hat{\beta}_0 + \hat{\beta}^\top x_i) > 1 \quad \Rightarrow \quad \hat{\xi}_i = 0
$$
So:
$$
y_i (\hat{\beta}_0 + \hat{\beta}^\top x_i) - 1 + \hat{\xi}_i \neq 0
\Rightarrow \hat{\gamma}_i = 0
$$
Therefore for **most observations**, $\hat{\gamma}_i = 0$. Only the **support vectors** have $\hat{\gamma}_i > 0$:
- These are the points that are either:
  - misclassified
  - on the margin
  - or on the wrong side of the margin

### Support vector machine
Let's have two predictors 
![[ChatGPT Image May 2025 from Matteo.png|150]]
Extending the support vector classifiers to the non linear spaces we have to separate the blu crosses from the red crosses. This leads at to main ideas:
- Can we **augment the feature space**, i.e., project the points onto a **higher-dimensional space**, so that observations become **linearly separable** in this space?
- Can we do this **without actually working** in the higher-dimensional space? yes, but this would be **computationally expensive**.

Lets have p features (predictors in statistics $\to X_{1}, ..., X_{p}$ ):  
We can build an **augmented space**: $X_{1}, X_{1}^{2}, X_{2}, X_{2}^{2},..., X_{p},X_{p}^{2} \in R^{2p}$, now is the double of the old space. The SVC in the augmented space will be:
$$
\max_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} \frac{1}{2} ||\underline{\beta}||^{2} \text{ where } ||\underline{\beta}||^{2} = \begin{pmatrix} \beta_{1}^{(1)} \\ \vdots \ \beta_{p}^{(1)} \\ \beta_{1}^{(2)} \\ \vdots \\ \beta_{p}^{(2)} \\ \end{pmatrix}^{\top}\begin{cases} x_{1} \\ \vdots \\ x_{p} \\ x_{1}^{2} \\ \vdots \\ x_{p}^{2} \\ \end{cases}
$$
such that:
- The soft margin ensures that each point is correctly classified by a margin of at least $1- \epsilon$ 
$$
y_{1}\left(\beta_{0}+ \sum_{}^{}\beta_{j,1}x_{j,1} + \sum_{}^{}\beta_{j,1}x_{j,1}^{2}\right)≥ 1 - \epsilon_{i}, i = 1,...,n
$$
- The slack variable $\epsilon_{i} ≥ 0, \text{for i = 1,...,n}$
- The sum violation is limited by C $\sum_{i=1}^n \xi_i \leq C$
Now in this augmented space the decision surface is linear $\underline{z} \in R^{2p}:\beta_{0}+ \underline{\beta}^{t}\underline{z} = 0$, but quadratic in an orthogonal space $\underline{x} \in R^{2}$. now the hyperspace is defined by:
$$\beta_{0}+ \sum_{}^{}\beta_{j}x_{i,j} + \sum_{}^{}\beta_{j}x_{i,j}^{2} = 0$$
This equation separates the space of observations into two classes, but does so in a new nonlinear feature space, where each feature has been potentially transformed (e.g., with squares).
Could become more complex by adding:
- Interaction terms
- more degree
but more term is added more the vector will become higher so with a longer optimization problem

Recall of [[#Support vector classifier]] 
In a **Support Vector Classifier (SVC)**, the decision surface is **linear** and given by:
$$\mathbf{x} \in \mathbb{R}^{p}, \quad f(\mathbf{x}) = 0$$
where the decision function is $$f(\mathbf{x}) = \beta_0 + \boldsymbol{\beta}^\top \mathbf{x} = \beta_0 + \sum_{i=1}^n \gamma_i y_i \mathbf{x}_i^\top \mathbf{x}$$
The dual problem to solve it in gamma
$$
f(\mathbf{x}) = \beta_0 + \sum_{i=1}^n \gamma_i y_i \langle \mathbf{x}_i, \mathbf{x} \rangle
$$
The $\gamma_{i}$ are estimated by solving the dual optimization problem:
$$\max_{\boldsymbol{\gamma}} \sum_{i=1}^{n} \gamma_{i} - \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} \gamma_i \gamma_{j} y_{i} y_{j} \langle \underline{x}_{i}, \underline{x}_{j} \rangle$$
This is written entirely in terms of **dot products** between observations $\underline{x}_{i}, \underline{x}_{j}$. Both **estimation** and **prediction** only require **inner products** between input vectors.
![[augmented_space.png]]

Linear decision surface
$$
\begin{align}
f(\underline{x}) &= \beta_{0} + \underline{\beta}^{t} \phi(\underline{x})\\
&\underline{\beta} = \sum_{i=1}^{n} \gamma_i y_i \phi(\underline{x}_{i})\\
f(\underline{x}) &= \beta_0 + \sum_{i=1}^{n} \gamma_{i} y_{i} \phi(\underline{x}_i) \phi(\underline{x}_{j})
\end{align}
$$
The Estimation of $\underline{\gamma}$ will use $\phi(\underline{x}_i) \phi(\underline{x}_{j})$ but this is computationally expensive so instead of use the inner product, we will implement the kernel trick.
The kernel trick allow us to substitute the inner product with the kernel, so we find a function (kernel) which is defined in the input space but which act as a dot product in some higher dimensional space $R^{n}$ for some map $\phi$

**Mercer's Theorem** provides the properties that a function must satisfy in order to be a valid kernel. Using one such kernel, we substitute the inner product in the transformed space, $\phi(\underline{x}_{i})\phi(\underline{x}_{j}) \implies k(\underline{x}_{i}, \underline{x}_{j})$. The ==decision function== becomes:
$$f(\underline{x}) = \beta_0 + \sum_{i=1}^n \gamma_i y_i \, k(\underline{x}_i, \underline{x})$$
This substitution applies also in the estimation of $\gamma$, making the model entirely kernel-based.

The coefficient $\underline{\gamma}$ is estimated by $$\max_{\boldsymbol{\gamma}} \sum_{i=1}^{n} \gamma_{i} 
- \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} \gamma_i \gamma_j y_i y_j \, k(\underline{x}_i, \underline{x}_j)
$$
Each kernel k gives s sum, so there are a family of model

#### Most common kernel 
- **Linear kernel**: $k(\underline{x}, \underline{z}) = \underline{x}^\top \underline{z}$
	- corresponds to the standard dot product so equivalent to the standard linear SVC
	- No transformation $\phi(\cdot)$ is applied
- **Polynomial Kernel**: $k(\underline{x}, \underline{z}) = (1 + \underline{x} \underline{z})^m$
	- m degree is a tuning parameter it define the polynomial degree
Example of polynomial kernel (m=2)
$$
\begin{align}
k(\underline{x}, \underline{z}) &= \left(1 + \sum_{i=1}^{p} x_{i} z_{i} \right)^{2} = \phi(\underline{x})\phi(\underline{z})\text{ find the } \phi \\
&= \sum_{j,k=1}^{p} (x_{j} x_{k})(z_{j} z_{k}) + \sum_{j=1}^{p} (\sqrt{2} x_{j})(\sqrt{2} z_{j}) + 1 \\
\end{align}
$$
The Mapping $\underline{x}$:
$$
\begin{align}
\phi(\mathbf{x}) =[ &x_{1}^{2},\ x_{1} x_{2},\ \dots,\ x_{p}^{2} \text{ as } p^{2} \text{quadratic terms}, \\
&\sqrt{2} x_1,\ \dots,\ \sqrt{2} x_{p} \text{ as } p \text{ linear terms scaled by } \sqrt{2}
\\ & 1] \text{ as constant} \\
\end{align}
$$
$$
k(\underline{x}, \underline{z}) = \phi(\underline{x})\phi(\underline{z}), \ \phi: \mathbb{R}^p \rightarrow \mathbb{R}^{p^2 + p + 1}
$$
the decision surface is quadratic: $\mathbf{x} : f(\mathbf{x}) = 0$ explicitly: $\underline{x} : \beta_0 + \beta^{t} \phi(\underline{x}) = 0$

- **Radial Basis Function (RBF) Kernel**: $k(\mathbf{x}, \mathbf{z}) = \exp \left( -\gamma \sum_{j=1}^{b} (x_{j} - z_{j})^{2} \right) \text{for } \gamma > 0$
	- $\gamma = \frac{1}{2\sigma^{2}}$ Gaussian kernel. Is an hyperparameter, how local is the effect of a point
		- $\gamma$ big: 
	- local $\underline{z}$ far from $\underline{x} \to k(\underline{z},\underline{x})$ prediction is based on $\underline{x}_{i}$ points closest to $\underline{x}$ (vede solo punti vicini, perché i punti lontani hanno un contributo trascurabile.)

#### Application 
1) Choose a Kernel 
2) The function will do optimization based on $k(\underline{x}_{i}, \underline{z}_{j})$ for $\binom{n}{2}$ values
The tuning parameter should be carefully selected by cross-validation
- Linear $C(\mu)$
- Polynomial $\mu, m$
- Radial Basis Function $\mu, \gamma$

### More than two classes
For more than two classes there are no obvious generalization, but we can use two potential approach:
- one versus the rest
- one versus one

#### One versus the rest
For a classification task with k classes, we train k **binary classifiers**,  
where each classifier learns to distinguish **class c** vs **not class c**.
Given a new observation $\underline{x}^{*}$, classify it by choosing the class with the **largest** value of:
$$
|f_c(\underline{x}^{*})|
$$

That is, choose the classifier with the strongest (most confident) response.
Potential issues:
- **Ambiguity** when multiple $f_{c}(\underline{x}^{*})$ are similar in magnitude
- **Unbalanced classes** can lead to biased classifiers

#### One versus one
For a classification task with k classes we fit $\binom{k}{2} = \frac{k(k-1)}{2}$ binary classifier, one for each pair of classes
- Classify $\underline{x}^{*}$  to the class with the largest number of votes
- computationally more expensive

## Support vector classifier vs logistic regression 
In this chapter will be defined the loss for the both the methods
- Support vector classifier $\implies$ [[#Hinge loss]]
- Linear regression $\implies$ Logistic Loss
### Hinge loss
Starting from the alternative formulation of the SVM problem:
$$
\max_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} \frac{1}{2} ||\underline{\beta}||^{2}
$$
such that 
$$
\begin{align}
&C_{1}: y_{i}(\beta_{0} + \underline{\beta}^{t}\underline{x}_{i}) ≥ 1-\epsilon_{i}\\ 
&C_{2}:\epsilon_{i}≥ 0\\
&C_{3}: \sum_{}^{} \epsilon_{i} ≤ C
\end{align}
$$

Indeed if we look in depth to the constraints $C_{1}$ and $C_{2}$:
$$
\begin{cases}
\epsilon_{i} \geq 1 - y_{i}(\beta_{0} + \boldsymbol{\beta}^{t} \mathbf{x}_{i}) \\
\epsilon_{i} \geq 0
\end{cases}
\quad \Rightarrow \quad 
\epsilon_i \geq \max\{0, 1 - y_i(\beta_0 + \boldsymbol{\beta}^T \mathbf{x}_i)\}

$$

**Hinge loss definition**
The hinge loss define how much a point is violating the margin
Now, define the hinge loss function as:

$$
[a]_+ := \max\{0, a\}
$$

So the slack variable becomes:
$$
\epsilon_i = [1 - y_i(\beta_0 + \underline{\beta}^t \underline{x}_i)]_+
$$
$$
\sum_{i = 1}^{n} [1 - y_{i}(\beta_{0} + \underline{\beta}^{t} \underline{x}_{i})] ≤ \sum_{i=1}^{n} \epsilon_{i} ≤ C
$$
So the original problem is equivalent to 
$$
\min_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} \frac{1}{2} ||\underline{\beta}||^{2} \text{ s.t. } \sum_{i = 1}^{n} [1 - y_{i}(\beta_{0} + \underline{\beta}^{t} \underline{x}_{i})]_{+} ≤ C 
$$
Regularized formulation (Lagrangian)
$$
\min_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} \frac{1}{2} ||\underline{\beta}||^{2} + \gamma \sum_{i = 1}^{n}[1 - y_i(\beta_0 + \underline{\beta}^{t} \underline{x}_i)]_{+}\text{ with } \gamma > 0 \text{ as tuning parameter}
$$

**How to obtain the ridge form**
$$\left(\min_{\beta_{0}, \underline{\beta}, \underline{\epsilon}} \frac{1}{2\gamma} ||\underline{\beta}||^{2} + \cancel{\gamma} \sum_{i = 1}^{n}[1 - y_{i}(\beta_0 + \underline{\beta}^{t} \underline{x}_i)]_{+}\right) \cancel{\frac{1}{\gamma}}$$

If we adjust the function as
$$
\min_{\beta_{0}, \underline{\beta}, \underline{\epsilon}}\sum_{i = 1}^{n}[1 - y_{i}(\beta_0 + \underline{\beta}^{t} \underline{x}_{i})]_{+} + \frac{1}{2\gamma} ||\underline{\beta}||^{2}
$$
So given $\frac{1}{\gamma} = \lambda$ we can rewrite it as 
$$
\min_{\beta_{0}, \underline{\beta}, \underline{\epsilon}}\sum_{i = 1}^{n}[1 - y_{i}(\beta_0 + \underline{\beta}^{t} \underline{x}_{i})]_{+} + \frac{λ}{2} ||\underline{\beta}||^{2}
$$

This part $\frac{\lambda}{2} ||\underline{\beta}||^{2}$ remind a ridge regression. in fact comparing the ridge $\sum_{}^{}(y-\underline{\beta}^{t}\underline{x})^{2} + λ||\underline{\beta}||^{2}$ (with his relative penalty), there different is in the loss, in fact of the ridge use the MSE, the SVM use the **hinge loss**

Going deeper in the hinge loss classification meaning the hinge loss: 
$$
L(y, f(x)) =
\begin{cases}
0 & \text{if } 1 - y f(x) \leq 0 \ \Rightarrow \ y f(x) \geq 1  \\
1 - y f(x) & \text{if } 1 - y f(x) > 0 \ \Rightarrow \ y f(x) < 1 
\end{cases}
$$

Where $f(x) = \beta_0 + \underline{\beta}^T \underline{x}$  is the prediction score (before applying any threshold). And:
- When $y f(x) \geq 1$, the observation is correctly classified **and lies beyond the margin** → no loss.
- When $y f(x) < 1$, the observation is **either within the margin or misclassified**, and the loss is **proportional to the distance from the margin**.

### Logistic loss
It does not have a ridge penalty by default, but we can add it.

Loss (negative log-likelihood)

$$
\text{Loss} = -\sum_{i=1}^{n} \log P(C_{i} \mid \underline{x}_{i})\text{ minus of log-likelihood}
$$

We want to rewrite it as: $L\left(y_i; f(\underline{x}_i)\right) \quad \text{with } y_i \in \{-1, 1\}$

Logistic Loss (by cases)

$$
L(y_i; f(\underline{x}_i)) =
\begin{cases}
- \log P(1 \mid \underline{x}_i) & \text{if } y_i = 1 \\
- \log (1 - P(1 \mid \underline{x}_i)) & \text{if } y_i = -1
\end{cases}
$$

In logistic regression, the predicted probability is:

$$
P(1 \mid \underline{x}_i) = \frac{e^{\beta_0 + \underline{\beta}^T \underline{x}_i}}{1 + e^{\beta_0 + \underline{\beta}^T \underline{x}_i}}
$$

Now consider each case to plug:
- If $y_{i} = 1$:
$$
-\log P(1 \mid \underline{x}_i) = \log\left( \frac{1}{P(1 \mid \underline{x}_i)} \right)
= \log\left(\frac{1 + e^{\beta_0 + \underline{\beta}^{T} \underline{x}_{i}}}{e^{\beta_0 + \underline{\beta}^{T} \underline{x}_{i}}}\right) = \log\left(1 + e^{- (\beta_0 + \underline{\beta}^T \underline{x}_i)}\right)
$$
- If $y_i = -1$:
$$
-\log(1 - P(1 \mid \underline{x}_{i})) =- \log\left(\frac{1 + e^{\beta_0 + \underline{\beta}^{T} \underline{x}_{i}}}{e^{\beta_0 + \underline{\beta}^{T} \underline{x}_{i}}}\right) =\log\left(1 + e^{\beta_0 + \underline{\beta}^T \underline{x}_i}\right)
$$

Unified log-loss expression
We can write both cases compactly as:

$$
L(y_i, f(\mathbf{x}_i)) = \log\left(1 + e^{-y_i f(\mathbf{x}_i)}\right)
$$

where $f(\underline{x}_i) = \beta_0 + \underline{\beta}^T \underline{x}_i$.

### hinge loss vs Log loss
- **Log-loss** is smooth and convex.
- **Hinge loss** is piecewise linear and zero when the prediction is beyond the margin.

A visual comparison:
![[Pasted image 20250504233428.png|300]]
- Log-loss: $\log(1 + e^{-y f(x)})$
- Hinge loss: $[1 - y f(x)]_+$



