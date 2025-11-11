# Bayesian inference
random variable Z, $[x_{i}]$
$f(z; \underline{\theta})$ distribution of Z $[x_{i}|Y = j]$
Data: $z_{1},..., z_{n}$

Aim is to estimate $\underline{\theta}$ from the data
The most common approach is maximising via the liklyhood:
$$
L(\underline{\theta}) = \prod_{i = 1}^{n}f(z; \underline{\theta})
$$
then 
$$
\hat{\underline{\theta}} = \arg max L(\underline{\theta})
$$
In doing this, we see $\underline{\theta}$ as unknown but fixed constants

In bayesian inference, $\theta$ is a random variable which has a prior distribution (knowledge about $\underline{\theta}$ before we see the data):
$$
h(\underline{\theta}) \text{ prior distribution}
$$

Given the data, we update this distribution -> posterior distribution (combining prior knowledge and information on data)
$$
g(\underline{\theta}|\text{data}) \text{ Posterior distribution}
$$

In practice 
$$
g(\underline{\theta}|\text{data}) = \frac{f(\text{data}|\underline{\theta})h(\underline{\theta})}{p(\text{data})} \propto f(\text{data}|\underline{\theta})h(\underline{\theta}) = L(\theta) \times h(\theta)
$$
so il a likelihood $\times$ the prior distribution. So i don't have to maximise no more. 

**Posterior Distribution**  
From my method, I obtain the entire posterior distribution, which allows me to summarize it using either the mean $E[\underline{\theta}|data]$ or the mode, depending on the context. These values serve as point estimates that condense the information from the distribution. While the frequentist framework uses confidence intervals to express uncertainty, the Bayesian approach refers to these as _credible intervals_. In practice, I begin with a prior distribution that reflects my initial beliefs, and then update it with observed data using a model, resulting in the posterior distribution.

![[Screenshot 2025-03-25 at 12.24.41.png|300]]

The bayesian do the sensitivity analysis. 

There are simple cases where i know the shape of this distribution $g(\underline{\theta}|\text{data})$ is known. However, in must cases, we have no knowledge about g.
In this case, there is the markov chain montecarlo sample methond, which is a procedure that gives sample of this distribution. There can be computaionally expensive. 

**Example Beta - Binomial**
Z is binary $[x_{i} \text{ binary }, X_{i}|Y=j]$ 
$Z \sim \text{Bernulli}(\theta)$
$z_{1}, ..., z_{n}$ data

So
$$L(\theta) = \prod_{i = 1}^{n}\theta^{z_{i}}(1-\theta)^{z_{i}} = \theta^{\sum_{i = 1}^{n} z_{i}}(1-\theta)^{n- \sum_{i = 1}^{n}z_{i}}$$ In classic / frequentist methods, 
$$
\hat{\theta}^{MLE} = \frac{\sum_{i = 1}^{n}x_{i}}{n} \text{ empirical frequencies}
$$

In bayes inference we need to set a prior distribution:
$$
h(\theta) = \frac{\Gamma[\alpha + \beta]}{\Gamma[\alpha] \Gamma[\beta]} \theta^{\alpha-1} (1-\alpha)^{\beta-1} \ \ \ \ 0≤\theta≤1
$$
with $\alpha$ and $\beta$ fixed accordings to prior knowledge

Then i combine with prior (so likelihood $\times$ prior)
$$
\begin{align}
g(\theta|\text{data}) &\propto L(\theta)h(\theta)\\
&= \theta^{\sum_{i = 1}^{n} z_{i}}(1-\theta)^{n- \sum_{i = 1}^{n}z_{i}} \times \theta^{\alpha-1} (1-\alpha)^{\beta-1} \\
&= \theta^{\sum_{i = 1}^{n} z_{i}+ \alpha -1}(1-\theta)^{n - \sum_{i = 1}^{n} z_{i}+ \beta -1}
\end{align}
$$We recognize $g(\theta | data)$ as a beta distribution with 
- $\alpha* =\sum_{i = 1}^{n} z_{i}+ \alpha$ 
- $\beta* = n- \sum_{i = 1}^{n} z_{i}+ \beta$ 
Technically we say that the beta prior is conjugate with the binomial likelihood.  


**Choice of prior** $\alpha, \beta$
$h(\theta) \propto \theta^{\alpha-1} (1-\alpha)^{\beta-1}$
$E[\theta] = \frac{\alpha}{\alpha + \beta}$
It is common to set $\alpha = \beta = 1$ (no knowledge about $\beta$), then 
$$
g(\theta|\text{data}) \sim Beta\left(\sum_{i = 1}^{n} z_{i}+ \alpha +1, n- \sum_{i = 1}^{n} z_{i}+ \beta+1 \right)
$$
So 
$$
\hat{\theta}^{\beta} = \frac{\sum_{i = 1}^{n} z_{i}+ \alpha +1}{n+2}
$$

Compare with 
$$
\hat{\theta}^{MLE} = \frac{\sum_{i = 1}^{n} z_{i}}{n}
$$