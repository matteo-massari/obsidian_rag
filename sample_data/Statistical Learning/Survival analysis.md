# Survival Analysis
In a Regression kind setting the response is T (because in most situation there is a time until the event occurs) in a medical example the T is time until death. 

> Survival allow us to take in account missing data

**Example**
A five-year medical study is conducted in which patients have been treated for cancer.

**Aim**: Predict patient survival time using features such as:
- Patient covariates
- Type of treatment
- etc.


Complication is that some (or many) of the patients may have **survived till the end** of the study.  These patients have a survival time that is called **_censored_**.
All we know is that it is **at least 5 years**, but we do not know its exact value. other patients may have **dropped out** of the study.

**Survival analysis** provides methods for estimating the **exact time of death** (or failure/event). There are many other application :
1. Modelling churn: `T = Time until cancellation`
2. Modelling failure time of a product (Reliability studies)
3. The variable can also be **not "time"**
4. Time is naturally **right-censored**, but you can also have cases left - sensoring such as:
	- *Left sensoring*: pregnancy studies, where measurement start 250 days after conception and T = "Time until the end of the pregnancy"
	- *Interval Censoring*


==Different distribution can be used to model the survival probability:==
- PDF: $f(t)$
- CDF: $F(t)=P(T≤t)$
- *Survival curve*: is used for model the probability T = "time until an event occurs"
	$S(t) = P(T>t) = 1-P(T≤t) = 1- F(t)$
- Hazard Function (or rate): rate of dying in the next instant at having survived up to time r

$$
\begin{align}
h(t) =& \lim_{\Delta t\to 0} \frac{P(t<T≤t+\Delta t|T>t)}{\Delta t} \\
=& \lim_{\Delta t\to 0} \frac{P(t<T≤t+\Delta t \cap T>t)}{\Delta t} \\
=& \lim_{\Delta t\to 0} \frac{1}{S(t)} \frac{P(t<T≤t+\Delta t)}{\Delta t} \\
=& \frac{1}{S(t)} \lim_{\Delta t\to 0} \frac{P(t<T≤t+\Delta t)}{\Delta t}
\end{align}
$$

![[Screenshot 2025-06-26 at 15.52.32.png|300]]

So the whole function will be: $$ = \frac{f(t)}{S(t)}$$
The function are all equivalent and can be all used for modelling
$$h(t) = \iff  f(t) \iff F(t) \iff S(t)$$

Censored data
## Model survival data
There are different ways of modelling survival data:
1) [[#Non-Parametric]] 
2) [[#Parametric]] has an hazard constant in time
3) [[#Semi-Parametric]] has a variable hazard 

### Survival data
Survival data are formed as 
$$
(\underline{x}_{i}, y_{i}, \delta_{i})
$$

Where:
- $\underline{x}_{i}$: covariates
- $y_{i}$: response
- $\delta_{i}$: censoring
	- $\delta_{i} = 0$ Censored so $y_{i}$ is censoring time $T > y_{i}$
	- $\delta_{i} = 1$ not censored, so $y_{i}$ is the event time $T = y_{i}$

The theoretical variable that has y:
- T: survival time
- C: censoring time
What we observe is 
$$
y = \min (T,C) = \begin{cases} T \ \ T≤C \\ C \ \ t>C \text{ die after the end of the study}\end{cases}
$$
In addition we observe that 
$$
S = \begin{cases} 1 \ \ T≤C \\ 0 \ \ T> C \end{cases}
$$


### Non-Parametric
We estimate the Survival curve $S(t)$ via non-parametric approches:
- [[#Naive option]]
- [[#Kaplan - Meier]]

**Data**
![[Screenshot 2025-06-26 at 16.41.46.png]]
$\circ$ Dropped out but non for sickness motivation (moving in another country)
$\bullet$  Die 

The censored, will not get the curve down, but after the time they exit they will non more at risk

#### Naive option
Estimate:
$$
\begin{align}
S(\hat{y}_{3}) = P(T>y_{3}) = \frac{1}{3} \text{ remove the dropped out}
\end{align}
$$
$S(\hat{y}_{3}) = \frac{2}{5}$ count the dropped like the die, is not accurate

#### Kaplan - Meier
The better option is the Kaplan - Meier formalized is:

At time 1:
- 4 person are still alive out of 5
- also the 2 censored person are counted because there are still in the experiment, so here we consider also patient 2 and 4.
$$
s(\hat{y}_{1}) = P(\widehat{T>y_{1}}) = \frac{4}{5}
$$

At time 2:
$$
S(y_{2}) = P(\widehat{T>y_{2}}) = \frac{4}{5}
$$

At time 3:
$$
\begin{align}
S(\hat{y}_{3}) = P(t>y_{3}) =& P(T > y_{3} | T > y_{1})P(T > y_{1}) +  P(T > y_{3} | T ≤ y_{1}) \{\gets \text{ this is = 0}\}P(T ≤ y_{1}) \\
=& P(T > y_{3} | T > y_{1})P(T > y_{1}) \\
=& \frac{2}{3} \times \frac{4}{5} = \frac{8}{15} \\
\end{align}
$$
At time 4
Is the same of time 3

At time 5:
$$
S(\hat{y}_{5}) = P(t>y_{5}) = 0
$$

We can plot it with the Survival Curve
![[Screenshot 2025-06-26 at 17.04.36.png|300]]

Formally, for a generic dataset: $d_{1},..., d_{k}:$ death times
Let 
- $r_{k} =$ number of patients alive an in the study just before die at time t
	- with the censure is  $r_{k} - \text{cencored cases}$
- $q_{k} =$ the actual number of patients who die at time $d_{k}$

The the calculation can be formalized as 
$$
\begin{align}
S(d_{k}) =& P(T>d_{k}|T>d_{k-1}) P(T>d_{k-1}) \\
=& P(T>d_{k}|T>d_{k-1}) P(T>d_{k-1}) \times ... \times P(T>d_{2}|T>d_{1}) P(T>d_{1})
\end{align}
$$
is estimated using 
$$
P(T>d_j|T>d_{j-1}) = \frac{r_{j} + q_{j}}{r_{j}}
$$
The **Kaplan - Meier estimator**
$$S(\hat{d}_{k}) = \prod_{j = 1}^{k}\frac{r_{j} + q_{j}}{r_{j}}$$
For time $t \in (d_{k}, d_{k+1})$, set $S(\hat{t}) = S(\hat{d}_{k})$



### Parametric
We can also assume that the distribution of T has a certain parametric form. Then $f(t), F(t), S(t), h(t)$ depends on the parameter

We can do this parametric estimation with two different approach:
- [[#Likelihood]]
- [[#Regression]]

#### Likelihood
We estimare the parameter via the likelihood
Given n observation $(y_{i}, S_{i}), i = 1,...,n$
$$ 
\begin{align}
L &= \prod_{i = 1}^{n} f(y_{i})^{S_{i}} S(y_{i})^{1-S_{i}} \\
&= \prod_{i = 1}^{n} \left(\frac{f(y_{i})}{S(y_{i})}\right)^{S_{i}} S(y_{i})\\
&= \prod_{i = 1}^{n}h(y_{i})^{S_{i}} S(y_{i})
\end{align}
$$
The rate $h(y_{i}) = \left(\frac{f(y_{i})}{S(y_{i})}\right)$ is the hazard function 


*Exponential survival model*
The simplest approach: $T \sim Exp(\lambda)$ so:
- $f(t) = \lambda e^{-\lambda t}$
- $F(t) = 1 - e^{-\lambda t}$
- $S(t) = 1-F(t) = e^{-\lambda t}$
- $h(t) = \frac{f(t)}{S(t)} = \frac{\lambda e ^{- \lambda t}}{e ^{- \lambda t}} = \lambda$ is a constant

All of this function depends on λ that is estimated through the maximum likelihood 
$$
L = \prod_{i = 1}^{n} \lambda^{S_{i}} e^{-\lambda t} \to \hat{\lambda} \to S(\hat{t}) = e^{-\hat{\lambda} t} \text{ parametric estimation of } S(t)
$$
Calculation
$$
L = \prod_{i = 1}^{n} \lambda^{S_{i}} e^{-\lambda t}
$$
Log-likelihood:
$$
\begin{align}
l(λ) =& \sum_{i = 1}^{n}[S(t)_{i}\ln(λ) -λt] \\
=& \sum_{i = 1}^{n}S(t)_{i}\ln(λ) - \sum_{i = 1}^{n}λy_{i} \\
\end{align}
$$
Derivate respect to λ 
$$
\frac{d}{dλ} = \frac{\sum_{n}^{i = 1} S(t)_{i}}{λ} - \sum_{i = 1}^{n}y_{i}
$$
Then = 0
$$
\begin{align}
\frac{\sum_{n}^{i = 1} S(t)_{i}}{λ} - \sum_{i = 1}^{n}y_{i} &= 0 \\
\frac{\sum_{n}^{i = 1} S(t)_{i}}{λ} &= \sum_{i = 1}^{n}y_{i} \\
\sum_{n}^{i = 1} S(t)_{i} &= \sum_{i = 1}^{n}y_{i} \times λ \\
\frac{\sum_{n}^{i = 1}S(t)_{i}}{\sum_{i = 1}^{n}y_{i}} &= λ \\
\hat{\lambda} &= \frac{\sum_{n}^{i = 1}S(t)_{i}}{\sum_{i = 1}^{n}y_{i}} 
\end{align}
$$
So the estimation of λ is $$\hat{S(t)} = e^{-\hat{λ}t}$$

#### Regression
Predict T from a number of covariates $x_{1}, ..., x_{p}$

Non parametric approaches can be used in this case, but they require some stratification of the observation into groups.
For example, with one prediction $x = \text{"gender"}$, one could split the observation into Male/Female and obtain a non-parametric estimation of $S(t)$ within each group

![[Screenshot 2025-06-26 at 17.41.47.png|300]]

This is called the log-rank test, to check if difference is significant. It easier to account for covariates in a parametric approach. For example if is assumed an exponential distribution, we could make assumption in $\underline{x}$ like in GLM's we create a link function. 
$$
T|\underline{X} = \underline{x} \sim Exp(\lambda(x)) \text{ with } \log(\lambda(\underline{x})) = \beta_{0} + \underline{\beta^{t}}\underline{x}
$$
now we estimate the $\beta$ and not the $\lambda$ 

Maximum likelihood estimation of $\beta_{0}, \underline{\beta} = \hat{\lambda}$ for each $\underline{x}$. 
- Survival curve for each realization $\underline{x}$ of the covariates
- the most used model is webble

Exponential may be too restrictive in many application so weibull distribution of (with 2 parameter) is more reliable

### Semi-parametric

#### Cox - proportional Hazard model

We model the hazard function as
$$
\begin{align}
h(t|\underline{x}_{i}) = h_{0}(t) \exp\left(\sum_{j = i}^{p}x_{i,j}\beta_{j}\right), i = 1,...,n 
\end{align}
$$
- $\beta_{0}$ is not included because replaced by $h_{0}(t)$
- $h_{0}(t):$ arbitrary function of $t \to h_{0}(t)$ is the baseline hazard: $$h(t|x_{1} = 0, x_{2} = 0,..., x_{p} = 0) = h_{0}(t)$$ function for when all the predictors are = 0
	This GLE the same of Semi-Parametric, because a part is parametric and a part is non-parametric

$$
\begin{align}
\ln(h(t|\underline{x}_{i})) = \ln(h_{0}(t)) + \underline{\beta}^{t}\underline{x}_{j}
\end{align}
$$


It is called proportional hazard because:

![[Screenshot 2025-06-26 at 18.12.52.png]]

**Interpretation of coefficient**
A generic individual with covariate $\underline{x}_{i}$ has a hazard that is $h_{0}(t)$ times $\exp\{\underline{\beta}^{t}\underline{x}_{i}\}$:
- $\exp\{\underline{\beta}^{t}\underline{x}_{i}\}$ Global risk for patient i
- $\beta$ relative risk for patient i

Individual $\beta_{s}$ capture the effect of each covariate on the hazard, keeping all other variable fixed at a constant.
- $\beta_{j}>0 \implies e^{\beta_{j}} > 1$ Increase hazard of dying at any time t given that you have survived up to time t as an $x_{j}$ increase by one unit
- $\beta_{j}<0 \implies e^{\beta_{j}} < 1$ Decrease the probability  

**Partial likelihood**
$h_{0}(t)$ unknown and the likelihood cannot be evaluated

Cox develop a novel inference procedure by approximating the full likelihood with a partial  likelihood. 
We can look at the data generating process in a different procedure
![[Screenshot 2025-07-10 at 17.36.30.png]]

Data will be $\text{event} = (t, \text{patient})$
The likelihood can be written as:
$$
\prod_{K=1}^{k}g(t_{k})f(\text{patient}|t_{k})
$$
- $\prod_{K=1}^{k}$ event time
- $g(t_{k})$ density function: something happen at time $t_{k}$
- $f(\text{patient}|t_{k})$ patient dies at time $t_{k}$ given that something happen in time $t_{k}$

The idea is to ignore $g(t)$ and concentrate on the part of the likelihood that must depends on $\underline{\beta}$ 
$$
\begin{align}
PL(\underline{\beta}) =& \prod_{K=1}^{k}g(t_{k})f(\text{patient}|t_{k})\\
\text{Multinomial}
\\
=& \prod_{K=1}^{k} \frac{h_{0}(t_{k})\exp\{\underline{\beta}^{t}\underline{x_{k}} \}}{\sum_{K^{*} \in R(t_{k})}^{} h_{0}(t_{k}) \exp\{\underline{\beta}^{t}\underline{x_{k*}}\}} \\
=& \prod_{K=1}^{k} \frac{\cancel{h_{0}(t_{k})}\exp\{\underline{\beta}^{t}\underline{x_{k}} \}}{\sum_{K^{*} \in R(t_{k})}^{} \cancel{h_{0}(t_{k})} \exp\{\underline{\beta}^{t}\underline{x_{k*}}\}}
\end{align}
$$
- $PL(\underline{\beta})$ does not depends on $h_{0}(t)$
- Find the $\underline{\beta}$ that maximise $PL(\underline{\beta})$
- $h_{0}(t)$ can be estimated in posterior 

## Survival model for prediction
==Relative risk== can be calculated for new individual on some test set:
$$
\hat{\eta_{i}} = \hat{\beta_{1}}x_{i,1} + \hat{\beta_{2}}x_{i,2} + ... + \hat{\beta_{p}}x_{i,p}
$$

1) We can stratify patients into low / medium / high risk, 
2) check for covariate that are particularly patient prevalent in each group

To check how good is the model we can use the ==AUC==
Let's say we get to persons risk compared: 
$$\hat{\eta_{i*}} > \hat{\eta_{i}} \implies t_{i}> t_{i*}$$
So we are cheking if the prediction of two subject are coherent with the time. If there will not die in a choerent time the prediciotn is wrong. Also we need to check that they are censored. 

So we could use the AUC like 
$$
C = \frac{\sum_{i,i*:y_{i}>y_{i}*}^{} I(\hat{\eta_{i*}} > \hat{\eta_{i}})\delta_{i*}}{\sum_{i,i*:y_{i}>y_{i}*}^{} \delta_{i}*}
$$
With $\hat{\beta}$ fitted on training data

## Relational Event Model (REM)

Estensione della Survival Analysis per modellare **interazioni dinamiche** tra coppie di nodi \( (s, r) \) in una rete.


Studiare **il rischio che un'interazione tra \( s \) e \( r \) accada al tempo \( t \)**, in funzione di covariate.

### Hazard Function
$$h(t \mid x_{sr}(t)) = h_0(t) \cdot \exp(\beta^T x_{sr}(t))$$
-  $h(t)$: tasso istantaneo che si verifichi l'interazione \( s \to r \) al tempo \( t \)
- \( h_0(t) \): baseline hazard (può essere costante o lasciata libera)
- \( x_{sr}(t) \): covariate relative alla coppia \( (s, r) \) al tempo \( t \)
- \( \beta \): coefficienti da stimare


## 🧩 Esempi di Covariate

### 📊 Dyadic (a livello di coppia)
1. Differenza d'età tra \( s \) e \( r \)
2. Distanza fisica tra \( s \) e \( r \)

### 🔵 Node-level
3. Se \( s \) è studente o docente

### 🔁 Endogene (dipendono dalla rete)

4. **Reciprocità**:
$$x_{sr}(t) =
\begin{cases}
1 & \text{se } (r, s) \text{ è accaduto prima di } t \\
0 & \text{altrimenti}
\end{cases}$$

5. **Triadic Closure**:
$$x_{sr}(t) =
\begin{cases}
1 & \text{se } (s, k) \text{ e } (k, r) \text{ accaduti prima di } t \\
0 & \text{altrimenti}
\end{cases}$$


---

## ⚙️ Partial Likelihood (tipo Cox)

$$PL(\beta) = \prod_{k=1}^n \frac{\exp(\beta^T x_{s_k r_k}(t_k))}{\sum_{(s,r) \in R(t_k)} \exp(\beta^T x_{sr}(t_k))}$$


- Numeratore: hazard dell'interazione osservata al tempo \( t_k \)
- Denominatore: somma su tutte le possibili interazioni \( (s,r) \) a rischio in quel momento

⚠️ **Costosa**: \( R(t_k) \) può essere molto grande → si usa una strategia di campionamento.

---

## 📉 Nested Case-Control Sampling

### 💡 Idea:
Campionare **un sottoinsieme di non-eventi** dal rischio a ogni \( t_k \), per ridurre il costo computazionale.

La probabilità si approssima come:

$$
P\left( (s_k, r_k) \mid (s_k, r_k) \cup \text{negativi} \right)
$$

---

### Logistic Regression Approximation

Approssimando con un solo non-evento per ciascun evento:

$$\tilde{PL}(\beta) = \prod_{k=1}^n \frac{1}{1 + \exp(-\beta^T(x_{s_k r_k} - x_{s_k r'}))}$$


👉 È una **regressione logistica** con:
- Variabile dipendente binaria: evento vs. non-evento
- Regressori: differenza tra covariate del pairing osservato e del non-evento

---

## 📌 Riepilogo finale

- REM = estensione hazard-based per interazioni tra nodi
- Covariate possono essere esogene o endogene (da rete)
- Si può stimare:
  - Con partial likelihood (tipo Cox)
  - Con logistic regression + negative sampling (approccio più pratico)



## hazard
Il passaggio che porta al termine 1S(t)\frac{1}{S(t)} deriva dall'applicazione della **probabilità condizionata**, ed è centrale per la definizione della **hazard function** h(t)h(t), o **funzione di rischio istantaneo**.

---

### Punto di partenza: definizione di hazard function

La hazard function è definita come:

h(t)=lim⁡Δt→0P(t<T≤t+Δt∣T>t)Δth(t) = \lim_{\Delta t \to 0} \frac{P(t < T \leq t+\Delta t \mid T > t)}{\Delta t}

cioè: **la probabilità che l'evento accada nell'intervallo (t,t+Δt](t, t+\Delta t]**, **dato che non è ancora accaduto prima di tt**, divisa per la lunghezza dell'intervallo (quindi una "densità condizionata").

---

### Passaggio 1: formula della probabilità condizionata

Usiamo la definizione di probabilità condizionata:

P(A∣B)=P(A∩B)P(B)P(A \mid B) = \frac{P(A \cap B)}{P(B)}

Nel nostro caso:

- A=(t<T≤t+Δt)A = (t < T \leq t + \Delta t)
    
- B=(T>t)B = (T > t)
    

Quindi:

P(t<T≤t+Δt∣T>t)=P(t<T≤t+Δt∩T>t)P(T>t)P(t < T \leq t + \Delta t \mid T > t) = \frac{P(t < T \leq t + \Delta t \cap T > t)}{P(T > t)}

Ma l'intersezione (t<T≤t+Δt)∩(T>t)(t < T \leq t + \Delta t) \cap (T > t) è semplicemente (t<T≤t+Δt)(t < T \leq t + \Delta t), quindi:

=P(t<T≤t+Δt)P(T>t)= \frac{P(t < T \leq t + \Delta t)}{P(T > t)}

E poiché P(T>t)=S(t)P(T > t) = S(t), cioè la funzione di sopravvivenza (la probabilità che l'evento **non** sia ancora avvenuto fino al tempo tt), allora:

=P(t<T≤t+Δt)S(t)= \frac{P(t < T \leq t + \Delta t)}{S(t)}

---

### Passaggio 2: sostituzione nella definizione di hazard

Sostituendo nella definizione iniziale:

h(t)=lim⁡Δt→01Δt⋅P(t<T≤t+Δt)S(t)=1S(t)⋅lim⁡Δt→0P(t<T≤t+Δt)Δth(t) = \lim_{\Delta t \to 0} \frac{1}{\Delta t} \cdot \frac{P(t < T \leq t + \Delta t)}{S(t)} = \frac{1}{S(t)} \cdot \lim_{\Delta t \to 0} \frac{P(t < T \leq t + \Delta t)}{\Delta t}

---

### Conclusione

Il termine 1S(t)\frac{1}{S(t)} **entra perché stiamo condizionando sul fatto che l'evento non sia ancora accaduto a tempo tt**. È il denominatore della probabilità condizionata:

P(A∣B)=P(A)P(B)⇒1P(B)=1S(t)P(A \mid B) = \frac{P(A)}{P(B)} \Rightarrow \frac{1}{P(B)} = \frac{1}{S(t)}

---

Se vuoi, possiamo anche interpretarlo graficamente o numericamente su un esempio di distribuzione (es. esponenziale). Vuoi che lo faccia?