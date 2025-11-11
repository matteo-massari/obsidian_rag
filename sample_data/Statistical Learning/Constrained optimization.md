# Constrained optimization
We use constriend optimization to find a best parameter $\theta$ given some constrants. we have seen in some example as ![[Screenshot 2025-07-07 at 12.42.52.png]]
## Optimization with equality constraint
In general optimization problem can be written as (maximization or minimization):
$$
\min_{\underline{\alpha}}f(\underline{\alpha}) \text{ or } \max_{\underline{\alpha}}f(\underline{\alpha})
$$
is a subset to some constraint $h_{i}(\alpha) = 0, \ i=1,...,n$

### Lagrangian
The Lagrangian function is useful to optimize 
$$
L(\underline{\alpha}, \gamma) = f(\alpha) + \sum_{i = 1}^{n} \gamma_{i}h_{i}(\underline{\alpha})
$$
with $\gamma_{1},...,\gamma_{n}$ ( $\gamma$ as a vector of) Lagrangian multiplier 

The solution of the Lagrangian is the stationary point:
$$
\frac{\partial L}{\partial \underline{\alpha}} = 0, \frac{\partial L}{\partial \underline{\gamma}} = 0
$$
This set of equations says that the gradient respect to $\alpha$ should be set to 0 but also satisfy the constraint
$$
\frac{\partial L}{\partial \underline{\gamma}} \to \frac{\partial L}{\partial \gamma_{i}} = h_{i}(\underline{\alpha})
$$
$\alpha^{*}$ is the optimum that minimize $f(\underline{\alpha})$ and satisfies the constraint. 

**Example** Multinomial
X is categorical , G categories, Data $x_{1},x_{2},...,x_{i}$, $\sum_{i = 1}^{G}x_{i} = n$ with $x_{i} = \#$ observation in category i
$$
f(x) = \frac{n!}{\prod_{i = 1}^{G}x_{i}!}\prod_{i = 1}^{G}\theta^{x_{i}} \text{ with } \underline{\theta} = \begin{pmatrix}\theta_{1} \\ \vdots \\ \theta_{G}\end{pmatrix}, \ \sum_{i = 1}^{G} \theta_{1} = 1
$$
which $\sum_{i = 1}^{G} \theta_{1} = 1$ is the constraint 

Perform the log-likelihood
$$
\begin{align}
L(\theta) = \log(n!) - \log(\prod_{i = 1}^{G}x_{i}!) + \sum_{i=1}^{G}x_{i}\log(\theta_{i})
\end{align}
$$
Perform the Lagrangian optimization  (there is only one Lagrangian multiplier, because there is only one constraint) 
$$
L(\theta, \gamma) = \log(n!) - \log(\prod_{i = 1}^{G}x_{i}) + \sum_{i=1}^{G}x_{i}\log(\theta_{i}) + \gamma(\sum_{i = 1}^{\theta}-1)
$$
Perform the derivate for each $\gamma$ and for each $\theta$ (this has to be solved simultaneously)
$$
\frac{dL}{d \underline{\theta}} = \frac{x_{i}}{\theta_{i}} - \gamma = 0, i = 1,...,G
$$
$$
\frac{dL}{d \gamma} = 1-\sum_{i = 1}^{G}\theta_{i} = 0
$$
Put the derivate equal to 0
$$
\begin{align}
&\frac{x_{i}}{\theta_{i}} = \gamma \to \theta_{i} = \frac{x_{i}}{\gamma} \\
&\hat{\theta}_{i} = \frac{x_{i}}{n} \text{ for } i = 1,...,G \text{ observed frequencies}\\
\\
&\sum_{i = 1}^{G}\theta_{i} = 1 \\
&\text{Substitue with } \theta \\
&\sum_{i = 1}^{G} \frac{x_{i}}{\gamma} = 1  \\
&\hat{\gamma} = \sum_{i = 1}^{G}x_{i} = n
\end{align}
$$
## Optimization with equality and inequality constraint
Consider 
$$
\min_{\underline{\alpha}} f(\underline{\alpha}) 
$$
such that:
- $h_{i}(\underline{\alpha}) = 0, \ i=1,...,p$ (equality constraint)
- $g_{i}(\underline{\alpha}) ≤ 0, \ i=1,...,n$ (inequality constriant)
### Generalized Lagrangian 
Is an extension of the lagrangian, but for equality and inequality constraint
$$
L(\underline{\alpha}, \underline{\gamma}, \underline{\delta}) = f(\underline{\alpha}) + \sum_{i=1}^{p} \gamma_{i}h_{i}(\underline{\alpha}) + \sum_{i = 1}^{n} \delta_{i}g_{i}(\underline{\alpha})
$$
with $\underline{\gamma}, \underline{\delta}$ Lagrange multipliers
To justify this function, consider:
$$
\theta_{p}(\underline{\alpha}) = \max_{\begin{align}\gamma, \underline{\delta}\\ \delta_{i}≥ 0\end{align}} L(\underline{\alpha}, \underline{\gamma}, \underline{\delta})
$$
So the new problem become 
$$
\min_{\alpha} f(\alpha) = \min_{\alpha} \theta_{\mathcal{L}}(\alpha) = \min_{\alpha} \max_{\gamma, \delta \geq 0} \mathcal{L}(\alpha, \gamma, \delta)
$$

Consider if $\alpha$ does not satisfy the constraints, then:
1) at least one of the constraint is violated:
	- There is a nonzero equality constraint $h_i(\alpha) \neq 0$ 
	- There is a positive equality constraint $g_i(\alpha) > 0$
2) we can choose multiplayer $\gamma$, $\delta$ so that the violation can be “punished” severely as 
	- $\gamma$ bigger so that $\gamma_{i}h_{i}(\underline{\alpha})$ can grow
	- $\delta$ bigger so that $\delta_{i}g_{i}(\underline{\alpha})$ can grow
	such that $\mathcal{L}(\alpha, \gamma, \delta) \to +\infty \implies \theta_{\mathcal{L}}(\alpha) = + \infty$
3) The results is $\theta_{\mathcal{L}}(\alpha) = +\infty$ that implies this $\alpha$ will never be a minimum

This optimization problem can be solved via:
- Lagrange duality 

### Lagrange duality
Allow to resolve the optimization problem
#### Primal Problem

$$
\min_{\alpha} f(\alpha) = \min_{\alpha} \theta_{\mathcal{L}}(\alpha) = \min_{\alpha} \max_{\gamma, \delta \geq 0} \mathcal{L}(\alpha, \gamma, \delta)
$$

This is the original problem: minimize $f(\alpha)$ subject to constraints, expressed via the generalized Lagrangian.

#### Dual Problem

$$
\max_{\gamma, \delta \geq 0} \theta_D(\gamma, \delta) = \max_{\gamma, \delta \geq 0} \min_{\alpha} \mathcal{L}(\alpha, \gamma, \delta)
$$

In this formulation, the order of min and max is reversed: we first minimize over $\alpha$, then maximize over the Lagrange multipliers.
In general, the two problems are **not the same**. In particular:
$$
\max \min \leq \min \max
$$

This is known as **weak duality** — the dual objective is always a lower bound on the primal.

**When Are They Equal?**
The primal and dual problems are **equivalent** (i.e., strong duality holds) when:
- $f$ and $g_i$ are **convex functions**
- $h_i$ are **linear**

This is the case for the **Support Vector Classifier (SVC)**.
Under these conditions:
$$
\min \max = \max \min
$$
This is known as **strong duality**.

## Karush–Kuhn–Tucker (KKT) Conditions
We can apply the KKT conditions if strong duality holds. We use them to fully characterize the optimal solution of a constrained optimization problem (it simplify the lagrangian).

1) Stationarity $$\nabla \alpha ​L(\alpha*,γ∗,\delta∗)=0$$
2) Primal feasibility
$$
\begin{align}
- g_{i}(\alpha^{*}) \leq 0 \\
- h_{j}(\alpha^{*}) = 0
\end{align}
$$
3) Dual feasibility
$$\delta^{*}_{i}≤0$$

4) Complementary slackness condition

Consider $\alpha^*$, $\gamma^*$, $\delta^*$ as the optimum, and suppose:
$$
\theta_P(\alpha^*) = \theta_D(\gamma^*, \delta^*)
$$
Then, the **complementary slackness condition** holds:
$$
\delta_i^* \cdot g_i(\alpha^*) = 0 \quad \text{for } i = 1, \dots, n
$$
**Proof**
We start from:
$$
f(\alpha^*) = \min_{\alpha} \mathcal{L}(\alpha, \gamma^*, \delta^*) = \min_{\alpha} \left( f(\alpha) + \sum_{i=1}^p \gamma_i^* h_i(\alpha) + \sum_{i=1}^n \delta_i^* g_i(\alpha) \right)
$$

Since $\alpha^*$ is optimal, we have:

$$
\leq f(\alpha^*) + \sum_{i=1}^p \gamma_i^* h_i(\alpha^*) + \sum_{i=1}^n \delta_i^* g_i(\alpha^*)
$$

But at the optimum, the equality constraints satisfy $h_i(\alpha^*) = 0$ and (because of dual feasibility) $\delta_i^* \geq 0$ and $g_i(\alpha^*) \leq 0$, so:
$$
\Rightarrow \sum_{i=1}^n \delta_i^* g_i(\alpha^*) = 0
$$
Therefore:
$$
\delta_i^* \cdot g_i(\alpha^*) = 0 \quad \forall i
$$
 **Interpretation**
This implies:

- If $\delta_i^* > 0$ $\Rightarrow$ $g_i(\alpha^*) = 0$ (active constraint)
- If $g_i(\alpha^*) < 0$ $\Rightarrow$ $\delta_i^* = 0$ (inactive constraint)

This is the essence of **complementary slackness**.


