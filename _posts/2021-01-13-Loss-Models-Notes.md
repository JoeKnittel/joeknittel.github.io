---
title: Loss Models Notes
header-includes:
   - \usepackage{actuarialangle}
   - \usepackage{placeins}
   - \usepackage{bm}
output: 
  html_document:
    keep_md: true
---



<style>
  div.blue {background-color:#bbdced; border-radius: 10px; padding: 10px;}
  div.green {background-color:#c4f0bb; border-radius: 10px; padding: 10px;}
  div.purple {background-color:#bdbbed; border-radius: 10px; padding: 10px;}
  div.red {background-color:#fcd2e0; border-radius: 10px; padding: 10px;}
</style>

$\newcommand{\vect}[1]{\boldsymbol{#1}}$

# Useful Definitions and Formulas

<div class = "purple">

$$
  f(x) \text{ is the (probability) density function, or pdf} \\
  F(x) = \int f(x) \ dx \text{ is the (cumulative) distribution function, or cdf} \\
  S(x) = 1-F(x) \text{ is the survival function (i.e., probability of survival beyond } x\text{)} \\
  \begin{align}
    h(x) &= \frac{f(x)}{S(x)} \text{ is the hazard rate function} \\
    f(x) &= F'(x) = -S'(x) \\
  \end{align}
$$
</div><br>

<div class = "purple">

$$
  \begin{align*}
    &\sum_{x=0}^\infty \frac{k^x}{x!} = e^k \\ \\
    &{x \choose k} = \frac{x!}{(x-k)! \ k!} \\ \\
    &\Gamma(n) = (n-1)!, \quad &n \in \mathbb{N} \\ \\
    &\Gamma(n;x) = 1 - \sum_{j=0}^{n-1} \dfrac{x^j e^{-x}}{j!}, \quad &n \in \mathbb{N}
  \end{align*}
$$
</div>

<br>

# Chapter 3: Basic Distributional Quantities

## 3.1: Moments

#### kth Raw Moment

<div class = "blue">

$$
  \mu_k'= \text{E}(X^k) = \int_{-\infty}^{\infty}x^k f(x) \ dx = \sum_j x^k p(x_j)
$$

$$
  \textbf{Mean: } \mu_1' = \mu 
$$
</div>

#### kth Central Moment

<div class = "blue">

$$
  \mu_k = \text{E}[(X-\mu)^k] = \int_{-\infty}^{\infty} (x-\mu)^k f(x) \ dx = \sum_j (x_j-\mu)^k p(x_j) 
$$

$$
  \mu_2 = \text{Var}(X) = \sigma^2 
$$

$$
  \textbf{Coefficient of Variation: } \frac{\sigma}{\mu}
$$

$$
  \textbf{Skewness: } \gamma_1 = \frac{\mu_3}{\sigma^3}  
$$

$$
  \textbf{Kurtosis: } \gamma_2 = \frac{\mu_4}{\sigma^4}
$$

$$
  \mu_2 = \mu_2'-\mu^2
$$
</div> <br>

- The standard deviation $\sigma$ is a measure of how much the probability is spread out
- Skewness is a measure of asymmetry
- symmetric distribution has a skewness of zero
- Kurtosis measures the flatness of a distribution relative to a normal distribution
- Normal distribution has a kurtosis of 3
- Coefficients of variation, skewness, and kurtosis are all dimensionless
- Integrals for moments might not exist or be infinite

#### Excess Loss Variable and Left-Censored and Shifted Variable

<div class = "green">

$$
  \textbf{Excess Loss Variable: } Y^P = X-d, \quad\quad\text{given that $X>d$}
$$
$$
  \textbf{Mean Excess Loss Function: } e_X(d) = \text{E}(Y^P) = \text{E}(X-d|X>d) = \frac{\int_d^\infty S(x) \ dx}{S(d)}
$$

</div> <br>

- The mean excess loss function is the expected value of the excess loss variable
- The mean excess loss function is sometimes called the ***mean residual life function*** and also the ***complete expectation of life*** denoted by the symbol $\overset{\circ}e_d$
- This could be considered a ***left truncated and shifted variable***, since any values of $X$ below $d$ are not observed and $d$ is subtracted from the remaining values

<div class = "green">

$$
  \textbf{kth Moment of Excess Loss: } e_X^k(d) = \frac{\int_d^{\infty} (x-d)^k f(x) \ dx}{1-F(d)} = \frac{\sum_{x_j>d}(x_j-d)^kp(x_j)}{1-F(d)}
$$
</div>

<br>

<div class = "purple">

$$
  \textbf{Left-Censored and Shifted Variable: } Y^L = (X-d)_+= 
      \begin{cases}
      0, & X\leq d \\
      X-d, & X>d
      \end{cases}
$$

$$
  \text{E}[(X-d)^k_+] = \int_d^{\infty} (x-d)^k f(x) \ dx = \sum_{x_j>d} (x_j-d)^k p(x_j) 
$$
$$
  \text{E}[(X-d)^k_+] = e^k(d)[1-F(d)]
$$

</div>

<br>

- Excess loss variable does not exists when $X<d$, then increases, whereas the left-censored and shifted variable is flat at 0 for $X<d$, then increasing to the right. See below:

<center>
![expect loss vs. left-censored and shifted](Figures/1.png)
</center>

<div class = "blue">

$$
  \textbf{Limited Loss Variable: }  X \wedge u = 
  \begin{cases}
    X, & X<u \\
    u, & X\geq u
  \end{cases}
$$

$$
  \begin{split}
  \text{E}[(X \wedge u)^k] 
    = & \int_{-\infty}^u x^k f(x) \ dx + u^k[1-F(u)]\\
    = & \int_{-\infty}^0 x^k f(x) \ dx + \int_0^u x^k f(x) \ dx + u^k[1-F(u)] \\ 
    = & - \int_{-\infty}^0 kx^{k-1}F(x) \ dx + \int_0^u kx^{k-1} S(x) \ dx
  \end{split}
$$

$$
  \text{E}(X \wedge u) = -\int_{-\infty}^0 F(x) \ dx + \int_0^u S(x) \ dx
$$

</div>
<br>

- Limited loss variable is also sometimes called the ***right-censored variable***
- $E(X \wedge u)$ is called the ***limited expected value***

<center>
![](Figures/2.png) 

</center>

<br>

- Sum of the expected values of the limited loss (right-censored) and left-censored and shifted random variables is equal to the expected value of the original random variable!

## 3.2: Percentiles

<div class = "purple">

$$
  \text{The }\textbf{100p}\textit{th percentile}\text{ of a random variable is any value $\pi_p$ s.t. $F(\pi_p-\leq p \leq F(\pi_p))$} 
$$

</div>
<br>

- The 50th percentile, $\pi_{0.5}$, is called the ***median***
- Quantiles and percentiles are very similar (e.g., 0.7 quantile = 70th percentile)

## 3.3: Generating Functions and Sums of Random Variables

<div class = "red">

Let $X_i$ be a collection of payments made by the insurer and $S_k = \sum_{i}^k X_i$. Then the following are true:

1. $\text{E}(S_k) = \text{E}(X_1) + \dots + \text{E}(X_k)$
2. If $X_i$ are independent: $\text{Var}(S_k) = \text{Var}(X_1) + \dots + \text{Var}(X_k)$
3. If $X_i$ are independent and their first two moments meet certain conditions: $\lim_{k\to\infty} [S_k - \text{E}(S_k)]/\sqrt{\text{Var}(S_k)}$ has a normal distribution with mean 0 and variance 1  

- The limit in #3 above is called ***convergence in distribution***

</div> <br>

<div class = "green">

$$
  \begin{align}   
    \textbf{Moment Generating Function (mgf): } M_X(z) &= \text{E}(e^{zX}) \\
    \textbf{Probability Generating Function (pgf): } P_X(z) &= \text{E}(z^X) 
  \end{align}
$$

- $M_X(0) = 1$
- $M^{'}_X(0) = E(X) = \mu$
- $M^{''}_X(0) = E(X^2)$
- $\text{Var}(X) = \text{E}(X^2) - \text{E}(X)^2 = M^{''}_X(0) - [M^{'}_X\left(0\right)]^2$


</div> <br>

<div class = "green">
$$
  \begin{align}
    M_x(z) &= P_X(e^z) \\
    P_X(z) &= M_X(\ln z)
  \end{align}
$$

</div>

<br>

<div class = "green">

If $S_k = X_1 + \dots + X_k$ and $X_i$ are independent, then the following are true: 

$$
  M_{S_k}(z) = \prod_{j=1}^k M_{X_j}(z)       
$$ 

$$
  P_{S_k}(z) = \prod_{j=1}^k P_{X_j}(z)
$$

</div>


## 3.4: Tails of Distributions

### 3.4.1: Classification Based on Moments

<div class = "purple">

- The ***tail*** of a distribution (more properly, the right tail) is the portion of the distribution
corresponding to large values of the random variable

- Random variables that tend to assign higher probabilities to larger values are said to be ***heavier tailed***

- The existence of all positive moments indicates a (relatively) light right tail

- Some or all non-positive moments indicates a heavy right tail

- Pareto distribution has a heavy tail, whereas the gamma distribution has a light tail

- If a distribution does not have all its positive moments, then it does not have an $mgf$; however, the converse is not true

- Heavy-tailed behavior is typically associated with large values of quantities such as the coefficient of variation, the skewness, and kurtosis

</div>

### 3.4.2: Comparison Based on Limiting Tail Behavior

<div class = "blue">

If the ratio of two survival functions (or density functions) diverges towards infinity, then the numerator distribution is heavier-tailed:

$$
  \lim_{x \to \infty} \frac{S_1(x)}{S_2(x)} = \infty = \lim_{x \to \infty} \frac{f_1(x)}{f_2(x)} \rightarrow \text{distribution 1 is heavier-tailed}
$$
</div>

### 3.4.3: Classification Based on the Hazard Rate Function

<div class = "red">

- Distributions with decreasing (i.e., non-increasing; can be constant) hazard rate functions have heavy tails

- Distributions with increasing (i.e., non-decreasing; can be constant) hazard rate functions have light tails

</div>

### 3.4.4: Classification Based on the Mean Excess Loss Function

<div class = "green">

- If the mean excess loss function is increasing in $d$, the distribution is considered to have a heavy tail 

- If the mean excess loss function is decreasing in $d$, the distribution is considered to have a light tail 

- The statements from 3.4.3 imply the statements from 3.4.4, but not necessarily the other way around (e.g., a distribution with a decreasing mean excess loss function does not have to have an increasing hazard rate function)

$$
  \lim_{d \to \infty} e(d) = \lim_{d \to \infty} \frac{1}{h(d)} \quad \text{(if the limits exist)}
$$

</div>

### 3.4.5: Equilibrium Distributions and Tail Behavior

<div class = "purple">

Let $X \geq 0$ and $S(0) = 1$, then $1 = \int_0^\infty [S(x)/E(X)] \ dx$, and hence the following is a probability density function:

$$
  \textbf{Equilibrium Distribution: }f_e(x) = \frac{S(x)}{\text{E}(X)}, \quad x \geq 0
$$
$$
  \begin{align}
    S_e(x) &= \int_x^\infty f_e(t) \ dt = \frac{\int_x^\infty S(t) \ dt}{\text{E}(X)}, \quad x\geq0 \\
    h_e(x) &= \frac{f_e(x)}{S_e(x)} = \frac{S(x)}{\int_x^\infty S(t) \ dt} = \frac{1}{e(x)}
  \end{align}
$$  
</div> <br>

- The equilibrium distribution is also called the ***integrated tail distribution***

- Coefficient of variation $\geq 1 \rightarrow$ heavy tail (p.39 - review) 

## 3.5: Measures of Risk

<div class = "blue">

- ***Key risk indicators*** are numbers describing the level of exposure to risk (e.g., VaR, TVaR)

- ***Value at Risk (VaR)*** is a quantile of the distribution of aggregate losses

- ***Tail Value at Risk (TVaR)*** is more informative than VaR, and sometime goes by other names: Conditional Value at Risk (CVaR), Average Value at Risk (AVaR), Conditional Tail Expectation (CTE), and Expected Shortfall (ES)

- ***Standard deviation principle***: $\mu + k \sigma$ is a risk measure

</div>

### 3.5.2: Risk Measures and Coherence

- Risk measures are denoted by $\rho(X)$ (thought of as the amount of assets required to protect against adverse outcomes of risk $X$)

- $X+Y$ could represent the loss variable composed of loss variables from different departments (e.g., individual life, health, group life, etc.) 

<div class = "red">

A ***coherent risk measure*** is a risk measure $\rho(X)$ that has the following four properties for any two loss random variables $X$ and $Y$:

1. Subadditivity: $\rho(X+Y) \leq \rho(X) + \rho(Y)$
2. Monotonicity: $X \leq Y \rightarrow \rho(X) \leq \rho(Y)$
3. Positive Homogeneity: $\forall c \in \mathbb{R^+}: \rho(cX) = c\rho(X)$
4. Translation Invariance: $\forall c \in \mathbb{R}: \rho(X+c) = \rho(X) + c$

</div> <br>

<div class = "red">

- Subadditivity reflects the fact that there should be some diversification benefit from combining risks

- Monotonicity means that greater losses imply greater risk measures

- Positive homogeneity means that the risk measure is independent of the currency in which the risk is measured

- Translation invariance means the assets required for a certain outcome is exactly the value of that outcome

</div>

### 3.5.3: Value at Risk

<div class = "green">

- The ***Value at Risk (VaR)*** is the amount of capital required to endure, with a high degree of certainty, that the enterprise does not become technically insolvent.

- $VaR$ often called ***quantile risk measure***

- The degree of certain is arbitrary and can often be between 95% and 99.95%

- Positive values of variable $X$ represent adverse outcomes, or "losses"

- $VaR$ of $X$ is the 100*p*th percentile of the distribution of $X$: $VaR_p(X) = \pi_p$

- $VaR_p(X)$ is the value $\pi_p$ that satisfies: $Pr(X>\pi_p) = 1-p \rightarrow VaR_p(X) = F^{-1}(p)$

- $VaR$ is incoherent, in general, because it violates subadditivity

- For normal distribution, VaR is coherent

</div>

### 3.5.4: Tail Value at Risk

<div class = "purple">

The ***tail value at risk of $X$*** at the 100*p*% security level, $TVaR_p(X)$, is the average of all $VaR$ values above the security level, *p*:

$$
  TVaR_p(X) = \frac{\int_p^1 VaR_u(X) \ du}{1-p} = E(X|X>\pi_p) = VaR_p(X) + e(\pi_p)
$$
</div>

# 4: Characteristics of Actuarial Models

- The ***principle of parsimony*** states that the simplest model that adequately reflects reality should be used

### 4.2.1: Parametric and Scale Distributions

<div class = "blue">

- A ***parametric distribution*** is a set of distribution functions each member of which is determined by specifying one or more values called ***parameters***

- The most familiar parametric distribution is the normal distribution, with parameters $\mu$ and $\sigma^2$

- All distributions in Apprendices A and B are parametric

- More parameters implies more complex model

</div> <br>

<div class = "red">

Let $X \sim d$ (e.g., $X$ is a distribution of type $d$). Then, if $\forall c \in \mathbb{R}^+: cX \sim d$, the parametric distribution, $d$, is considered a ***scale distribution***

- Example 4.1 on p. 53 demonstrates this clearly

</div> <br>

<div class = "red">

Let $X \sim d(\alpha,\beta, \gamma, \dots)$ be a random variable with scale distribution $d$ with a parameter $\alpha$ <br>
Let $c \in \mathbb{R}^+$

A ***scale parameter***, $\alpha$, is a parameter for a scale distribution that satisfies the following:

- $cX \sim d(c\alpha, \beta, \gamma, \dots)$ 

- (e.g., multiplying a scale distribution multiplies the scale parameter(s), but not other parameters)

</div>

### 4.2.2: Parametric Distribution Families

- A ***parametric distribution family*** is a set of parametric distributions that are related in some meaningful way 
- (e.g., the transformed beta family: Pareto($\gamma=\tau=1$), Paralogistic($\tau=1, \gamma=\alpha$))

### 4.2.3: Finite Mixture Distributions

- A dental claim may be from filling, repair (e.g., crown), surgical procedure. Different modes for these possibilities, so a ***mixture model*** may work well to describe a random claim

<div class = "green">

$Y$, a mixture of $\{{X_1,X_2, \dots, X_k\}}$, is called a ***k-point mixture*** if:

$$
  F_Y(y) = a_1F_{X_1}(y)+a_2F_{X_2}(y) + \dots + a_kF_{X_k}(y) = \sum_i a_iF_{x_i}(y),
$$
where all $a_j>0$ and $\sum_i a_i = 1$

(i.e., a k-point mixture's distribution is a weighted sum of its component marginal distribution functions)

</div> <br>

<div class = "green">

A ***variable-component mixture distribution*** has a distribution function that can be written as:

$$
  F(x) = \sum_{j=1}^K a_j F_j(x), \quad \sum_{j=1}^K a_j = 1, \quad a_j>0, \quad j=1,\dots,K \quad K=1,2,\dots
$$

- See Example 4.5 (p. 56) for an example of a mixture-of-exponentials distribution

- Mixture-of-exponentials is a good all-purpose model

</div> <br>

<div class = "purple">

- A ***data-dependent distribution*** is at least as complex as the data or knowledge that produced it

- The ***empirical model*** is a discrete distribution based on a sample size $n$ that assigns probability $1/n$ to each data point (see Example 4.7 on p. 57)

- The empirical distribution is a data-dependent distribution

- The kernel smoothing model (Section 14.6) is another example of a data-dependent model

</div>


# 5: Continuous Models

- ***Positive support***: $F(0) = 0$ (exponential, gamma, pareto, lognormal)

## 5.2: Creating New Distributions

### 5.2.1: Multiplication by a Constant

<div class = "blue">

- Transformation applies inflation uniformly across all loss levels and is known as a ***change of scale*** (e.g., $Y=1.05X$)

- Let $X$ be a continuous random variable and $Y = \theta X$, with $\theta > 0$. Then:

$$
  \begin{align}
  F_Y(y) &= \Pr(Y \leq y) = \Pr\left(X \leq \frac{y}{\theta}\right) = F_X\left(\frac{y}{\theta}\right) \\
  f_Y(y) &= \frac{d}{dy} F_X\left( \frac{y}{\theta}\right) = \frac{1}{\theta} f_X \left( \frac{y}{\theta}\right)
  \end{align}
$$

- $\theta$ is a scale parameter

</div>

### 5.2.2: Raising to a Power

<div class = "red">

- Let $X$ be continuous  with $F_X(0) = 0$; let $Y=X^{1/\tau}$. Then:

$$
  \tau>0: \\
  \begin{align}
  F_Y(y) &= \Pr(Y \leq y) = \Pr(X \leq y^\tau) = F_X(y^\tau) \\
  f_Y(y) &= \frac{d}{dy}F_X(y^\tau) = \tau y^{\tau-1} f_X(y^\tau), \quad y>0 \\ \\
  \end{align}
$$
</div><br>
<div class = "red">
$$
\tau<0: \\
  \begin{align}
  F_Y(y) &= 1-F_X(y^\tau) = 1-F_X\left(y^{-\tau^*}\right) \\
  f_Y(y) &= -\tau y^{\tau-1}f_X(y^\tau) = \tau^*y^{-\tau^*-1}f_X\left(y^{-\tau^*}\right)
  \end{align}
$$
</div><br>
<div class = "red">
- When raising a distribution to a power, if $\tau>0$, the resulting distribution is called ***transformed***; if $\tau=-1$, it is called ***inverse***; and if $\tau<0$ (but not $-1$), it is called ***inverse transformed***

</div>
<br>

- The transformed exponential distribution with scale parameter is commonly known as the ***Weibull distribution*** and has cdf $F(y) = 1-e^{-(y/\theta)^\tau}$

<div class = "green">

$$
  \textbf{Incomplete Gamma Function:} \quad \Gamma(\alpha;x) = \frac{1}{\Gamma(\alpha)} \int_0^x t^{\alpha-1} e^{-t} \ dt \\
  \textbf{Gamma Function:} \quad \Gamma(\alpha) = \int_0^{\infty} t^{\alpha-1} e^{-t} \ dt = (\alpha-1)! \quad (\text{if }\alpha \in \mathbb{Z}^+)
$$

</div>


### 5.2.3: Exponentiation

<div class = "purple">

- Let $X$ be continuous, with $\forall x\in \mathbb{R}: f_X(x) > 0$. Let $Y = e^X$. Then, for $y>0$:

$$
  F_Y(y) = F_X(\ln y), \quad  f_Y(y) = \frac{1}{y} f_X(\ln y)
$$

</div> <br>

- If $X \sim \mathcal{N}(\mu, \sigma^2)$. Then, $Y = e^X$ has a ***lognormal distribution***


### 5.2.4: Mixing (<font color = "red">review</font>)

<div class = "blue">

- In the following theorem, the pdf $f_\Lambda(\lambda)$ plays the role of the discrete probabilities $a_j$ in the k-point mixture (i.e., this is the extension of the k-point mixture to the continuous case):

$$
  \begin{align}
  \textbf{Mixture Distribution:} \quad \\ \\ f_X(x) &= \int f_{X|\Lambda}(x|\lambda) f_\Lambda(\lambda) \ d\lambda \quad (\text{unconditional distribution/marginal pdf of } X)\\
  F_X(x) &= \int F_{X|\Lambda} (x|\lambda) f_\Lambda(\lambda) \ d\lambda \\
  \text{E}(X^k) &= \text{E}[\text{E}(X^k | \Lambda)] \\
  \text{Var}(X) &= \text{E}[\text{Var}(X|\Lambda)] + \text{Var}[\text{E}(X|\Lambda)]
  \end{align}
$$
</div>

<br>

- Mixture distributions tend to be heavy-tailed

- Letting $X|\Lambda$ have an exponential distribution with parameter $1/\Lambda$ and $\Lambda$ have a gamma distribution, the unconditional distribution of $X$, $f_X(x) = \dfrac{\alpha \theta^\alpha}{(x+\theta)^{\alpha+1}}$, the ***pareto distribution*** (see Example 5.4 on p. 66)

- Example 5.5 on p. 66 is also a good one

- Example 5.6 on p. 67 is tricky (gotta know Weibull and Gamma distributions well + transforms from 5.2.2)


### 5.2.5: Frailty Models (<font color = "red">review</font>)

- A ***frailty model*** is an important type of mixture distribution

- The frailty is meant to quantify uncertainty associated with the hazard rate

<div class = "red">

Let $A(x) = \int_0^x a(t) \ dt$. 

$$
  \begin{align*}
  \textbf{Conditional survival function of } X|\Lambda\textbf{:}& \quad S_{X|\Lambda}(x|\lambda) = e^{-\int_0^x h_{X|\Lambda}(t|\lambda) \ dt }= e^{-\lambda A(x)} \\
  \textbf{Moment generating function of frailty variable } \Lambda\textbf{:}& \quad M_\Lambda(z) = \text{E}(e^{z\Lambda}) \\
  \textbf{Marginal survival function:}& \quad S_X(x) = \text{E}\left[e^{-\Lambda A(x)}\right] = M_\Lambda[-A(x)] \\
  \textbf{Distribution function:}& \quad F_X(x) = 1-S_X(x)
  \end{align*}
$$

</div>

<br>

- The most common choice is gamma frailty, but other choices such as inverse Gaussian frailty are also used

### 5.2.6: Splicing

<div class = "green">

- One model governs the behavior of losses in some interval of possible losses while other models cover the other intervals

$$
  \textbf{k-component spliced distribution:} \quad f_X(x) = 
  \begin{cases}
    a_1f_1(x), & c_0<x<c_1, \\
    a_2f_2(x), & c_1<x<c_2, \\
    \vdots, & \vdots \\
    a_kf_k(x), & c_{k-1}<x<c_k, \\
  \end{cases}
$$

- Each component distribution must have all of its density on its given subdomain, and $\sum a_i = 1$

- The motivation for splicing is that the tail behavior may be inconsistent with the behavior for small losses

</div> <br>

<center>
![](Figures/3.png)


## 5.3: Selected Distributions and their Relationships

<center>

![](Figures/4.png)
<br> <br>

![](Figures/5.png)

</center>

<br>

<div class = "purple">

- If the goal is to model the maximum observation from a random sample, the inverse Weibull distribution is likely to be a good candidate

- The Pareto distribution is a good model for heavy-tailed losses

</div>


## 5.4: The Linear Exponential Family

<div class = "blue">

A random variable $X$ (discrete or continuous) has a distribution from the ***linear exponential family*** if its pdf may be parametrized in terms of a parameter $\theta$ and expressed as:

$$
  \begin{align}
  f(x;\theta) &= \frac{p(x)e^{r(\theta)x}}{q(\theta)} \\
  \text{E}(X) &= \mu(\theta) = \frac{q'(\theta)}{r'(\theta) q(\theta)} \\
  \text{Var}(X) &= \nu(\theta) = \frac{\mu'(\theta)}{r'(\theta)} 
  \end{align}
$$



- The function $p(x)$ depends on $x$ (not on $\theta$), and the function $q(\theta$) is a normalizing constant. 

- The support of the random variable must not depend on $\theta$

- The parameter $r(\theta)$ is called the ***canonical parameter*** of the distribution

</div> <br>

- Normal distribution is a member of the linear exponential family

- Gamma distribution is a member of the linear exponential family


# 6: Discrete Distributions

<div class = "red">

Let $N$ be a random variable representing the number of events (claims, losses, etc.):

$$
  \textbf{Probability Function: } \quad p_k = \Pr(N=k), \quad k=0,1,2,\dots
$$
</div><br>

<div class = "red">

$$
  \textbf{Probability Generating Function (pgf): }    \quad   P(z) = \text{E}(z^N) = \sum_{k=0}^\infty p_k z^k \\
  \begin{align}
  P'(1) &= \text{E}(N) \\
  P''(1) &= \text{E}[N(N-1)] \\
  P^{(m)}(0) &= m! \ p_m
  \end{align}
$$
</div>

## 6.2: The Poisson Distribution

<div class = "green">

$$
  \textbf{Poisson distribution: } \quad p_k = \frac{e^{-\lambda}\lambda^k}{k!} \quad k=0,1,2,\dots \\
  P(z) = \text{E}(z^x)= e^{-\lambda}\sum_{x=0}^\infty\frac{(z\lambda)^x}{x!} = e^{-\lambda} e^{z\lambda} = e^{\lambda(z-1)} , \quad \lambda>0 \\
$$

</div> <br>

<div class = "green">

$$
  \text{E}(N) = P'(1) = \lambda \\
  \text{E}[N(N-1)] = P''(1) = \lambda^2 \\
  \begin{align}
  \text{Var}(N) & =  \text{E}[N(N-1)] + \text{E}(N) - [\text{E}(N)]^2 \\
         & =  \lambda^2 + \lambda - \lambda^2  \\
         & = \lambda
  \end{align}
$$

</div>

<br>

<div class = "purple">

- Let $N_1, \dots,N_n$ be independent Poisson variable with parameters $\lambda_1,\dots,\lambda_n$. Then, $N=N_1+\dots+N_n$ has a Poisson distribution with parameter $\lambda_1+\dots+\lambda_n$

- (i.e., the sum of Poisson variables is Poisson with sum of parameters) <br>

</div><br>

<div class = "purple">

- Suppose that the number of events $N$ is a Poisson random variable with mean $\lambda$. Further, suppose that each event can be classified into one of $m$ types with probabilities $p_1,\dots,p_m$ independent of all other events. Then, the number of events $N_1,\dots,N_m$ corresponding to event types $1,\dots,m$, respectively, are mutually independent Poisson random variables with means $\lambda p_1,\dots,\lambda p_m$, respectively

</div>


## 6.3: The Negative Binomial Distribution

- Used extensively as an alternative to the Poisson distribution

- Has two parameters, so it has more flexibility in shape than Poisson

<div class = "blue">

$$
  \textbf{Negative Binomial Distribution: } \quad p_k = \Pr(N=k) = {k+r-1 \choose k} \left( \frac{1}{1+\beta}\right)^r \left( \frac{\beta}{1+\beta}\right)^k, \quad k\in\mathbb{N}, \ r>0, \  \beta>0 \\
$$

$$
  \begin{align}
  P(z) &= [1-\beta(z-1)]^{-r} \\
  \text{E}(N) &= r\beta \quad \text{Var}(N) = r\beta(1+\beta)
  \end{align}
$$


</div> <br>

- $Var > E(N)$; hence, if the observed variance is larger than observed mean, negative binomial would be a better candidate than the Poisson distribution $\left(Var = E(N)\right)$ 

- The ***geometric distribution*** is the special case of the negative binomial distribution when $r=1$

- Geometric distribution is kind of the discrete analog of the continuous exponential distribution (memoryless)

- "Given that there are at least $m$ claims, the probability distribution of the number of claims in excess of $m$ does not depend on $m$"

- Heavy tail vs. geometric distribution when $r<1$ and lighter tail when $r>1$

- It is possible to observe $N$, but not be able to observe $\lambda$; in this case, we have ***parameter uncertainty***

- If the parameter is a function of another random variable, $\lambda(\Lambda)$, we call $\Lambda$ the ***prior distribution***

- The parameters of prior distribution, $\Lambda$, are called ***hyperparameters***

- The mixed Poisson, with a gamma mixing distribution, is the same as a negative binomial distribution

- The Poisson distribution is a limiting case of the negative binomial distribution

## 6.4: The Binomial  Distribution

- Arises naturally in claim number modeling

- Variance smaller than mean

- Binomial distribution arises from $m$ independent individual events whose probability of occurring follows a ***Bernoulli distribution*** with $P(z) = 1+q(z-1)$, resulting in the following characteristics for the Binomial distribution:

- Sometimes useful due to its finite support (range of values for which there exist positive probabilities). For instance, there are a certain amount of insured people; therefore, there can be no more than that amount of claims

<div class = "red">

$$
  \begin{align}
  &\textbf{Binomial Distribution: } \\ \quad p_k = \Pr(N=k) = &{m \choose k} q^k(1-q)^{m-k}, \quad k=0,1,\dots,m \\ \\
  &P(z) = [1+q(z-1)]^m, \quad 0<q<1 \\
  &\text{E}(N) = mq, \\
  &\text{Var}(N) = mq(1-q)
  \end{align}
$$

</div>


## 6.5: The (a,b,0) Class

<div class = "green">

$$
  \textbf{(a,b,0) Class: } \quad p_k = \left( a+\frac{b}{k} \right) p_{k-1}, \quad k=1,2,3,\dots
$$

- Two parameters $a$ and $b$, and $p_0$ is defined recursively by the fact that the distribution must sum to 1

</div>

<br><center>
![](Figures/6.png)
</center>


## 6.6: Truncation and Modification at Zero

- Often times the probability at zero is really important, so we must pay special attention to the model fit at this point

<div class = "purple">

$$
  \textbf{(a,b,1) Class: } \quad p_k = \left(a + \frac{b}{k} \right) p_{k-1}, \quad k = 2,3,4,\dots
$$

</div><br>

- In the $(a,b,1)$ class, the recursion starts at $p_1$ rather than $p_0$ in the $(a,b,0)$ class

- $\sum_{k=1}^\infty p_k$ could be any number in the interval $[0,1]$, with the remainder of the probability at $k=0$

- If $p_0=0$, we call the distribution ***truncated*** or ***zero-truncated*** (e.g., zero-truncated Poisson = "ZT Poisson", zero-truncated binomial = "ZT binomial", etc.)

- ***Zero-modified*** distributions are modified $(a,b,0)$ class distributions, sometimes called ***truncated with zeros*** (e.g., zero-modified Poisson = "ZM Poisson", etc.)

- We use $p_k^T$ when talking about a zero-truncated distribution, and $p_k^M$ when talking about a zero-modified distribution

<div class = "blue">

$$
  \begin{align}
  P^M(z) &= \left( 1 - \frac{1-p_0^M}{1-p_0} \right)1 + \frac{1-p_0^M}{1-p_0} P(z)  \\
  p_k^M &= \frac{1-p_0^M}{1-p_0} p_k, \quad k = 1,2,\dots \\
  P^T(z) &= \frac{P(z) - p_0}{1-p_0} \\
  p_k^T &= \frac{p_k}{1-p_0}, \quad k = 1,2,\dots \\
  p_k^M &= (1-p_0^M) \ p_k^T, \quad k = 1,2, \dots \\ 
  P^M(z) &= p_0^M(1) + (1-p_0^M) \ P^T(z) 
  \end{align}
$$

</div> <br>

- ***Zero-inflated*** means that $p_0^M > p_0$

- $\text{Var}(\text{ZI Poisson}) > \text{E}(\text{ZI Poisson})$

- We can create the ***"extended" truncated negative binomial (ETNB)*** by allowing $r$ to be negative (recall that the negative binomial is an $(a,b,0)$ class distribution; hence, $r>0$)

<div class = "red">

The limiting case of this ETNB (as $r$ goes to $0$), is the logarithmic distribution:

$$
  \textbf{Logarithmic Distribution: } \quad p_k^T = \frac{[\beta/(1+\beta)]^k}{k \ln(1+\beta)}, \quad k=1,2,3,\dots
$$
$$
  P^T(z) = 1 - \frac{\ln[1-\beta(z-1)]}{\ln(1+\beta)}
$$

</div> <br>

<center>

![](Figures/7.png)

</center>


# 8: Frequency and Severity with Coverage Modifications

- $Y^L$ is a ***per-loss*** variable (can be zero) 

- $Y^P$ is a ***per-payment*** variable (cannot be zero)

- $Y^P = Y^L | Y^L > 0$

## 8.2: Deductibles

- Insurance policies are often sold with a ***per-loss deductible*** of $d$. When the loss, $x$, is at or below $d$, the insurance pays nothing. When the loss is above $d$, the insurance pays $x-d$.


<div class = "green">

An ***ordinary deductible*** modifies a random variable into either the excess loss or left-censored and shifted variable, depending on whether the deductible is per-payment or per-loss:

<center>

$$
  \begin{align}
  \textbf{Per-payment Variable: } \quad Y^P &= \begin{cases}
                                                \text{undefined}, & X \leq d, \\
                                                X-d, & X>d
                                              \end{cases}
                                  \\
  f_{Y^P}(y) &= \frac{f_X(y+d)}{S_X(d)}, \quad y>0 \\
  S_{Y^P}(y) &= \frac{S_X(y+d)}{S_X(d)} \\
  F_{Y^P}(y) &= \frac{F_X(y+d)-F_X(d)}{1-F_X(d)} \\
  h_Y^P(y) &= \frac{f_X(y+d)}{S_X(y+d)} = h_X(y+d)
  \end{align}
$$

</div> <br>

<div class = "green">

$$
  \begin{align}
  \textbf{Per-loss Variable: } \quad Y^L &= \begin{cases}
                                                0, & X \leq d, \\
                                                X-d, & X>d
                                              \end{cases}
                                  \\
  f_{Y^L}(y) &= f_X(y+d), \quad y>0 \\
  S_{Y^L}(y) &= S_X(y+d), \quad y \geq 0 \\
  F_{Y^L}(y) &= F_X(y+d), \quad y \geq 0 \\
  h_{Y^L}(y) &= h_{Y^P}(y) \quad \text{(undefined at } y=0)
  \end{align}
$$

</div> <br>

<div class = "green"> 

$$
  \begin{align}
  \textbf{Expected cost per loss: }& \quad \text{E}(X) - \text{E}(X \wedge d) \\
  \textbf{Expected cost per payment: }& \quad \frac{\text{E}(X) - \text{E}(X \wedge d)}{1-F(d)}
  \end{align}
$$

</div><br>

</center>

<div class = "purple">

***Franchise deductible***:  when the loss exceeds the deductible, the loss is paid in full. This modifies the ordinary deductible by adding the deductible when there is a positive amount paid:

$$
  \begin{align}
  Y^L &= \begin{cases}
          0, & X \leq d, \\
          X, & X>d
        \end{cases} \\
          Y^P &= \begin{cases}
          \text{undefined}, & X \leq d, \\
          X, & X>d
        \end{cases} 
  \end{align}
$$

</div> <br>

<div class = "purple">

$$
  f_{Y^L}(y) = \begin{cases}
              F_X(d), & y=0 \ \ \ \ \ \ \   \\
              f_X(y), & y>d
            \end{cases} \\
  S_{Y^L}(y) = \begin{cases}
              S_X(d), & 0\leq y\leq d \\
              S_X(y), & y>d
            \end{cases} \\
  F_{Y^L}(y) = \begin{cases}
              F_X(d), & 0\leq y \leq d \\
              F_X(y), & y>d
            \end{cases} \\
  h_{Y^L}(y) = \begin{cases}
              0, & 0<y<d \\
              h_X(y), & y>d
            \end{cases} \\
$$

<br> </div> <br>

<div class = "purple">

$$ 
  \begin{align}
    f_{Y^P}(y) &=  \frac{f_X(y)}{S_X(d)}, \quad y>d  \\
    S_{Y^P}(y) &= \begin{cases}
              1, & 0\leq y\leq d \\
              \dfrac{S_X(y)}{S_X(d)}, & y>d
            \end{cases} \\
  F_{Y^P}(y) &= \begin{cases}
              0, & 0\leq y \leq d \\
              \dfrac{F_X(y)-F_X(d)}{1-F_X(d)}, & y>d
            \end{cases} \\
  h_{Y^P}(y) &= \begin{cases}
              0, & 0<y<d \\
              h_X(y), & y>d
            \end{cases} \\
  \end{align}
$$

</div><br>

<div class = "purple"> 

$$
  \begin{align}
  \textbf{Expected cost per loss: }& \quad \text{E}(X) - \text{E}(X \wedge d) +d[1-F(d)] \\
  \textbf{Expected cost per payment: }& \quad \frac{\text{E}(X) - \text{E}(X \wedge d)}{1-F(d)} + d
  \end{align}
$$

</div><br>

- Examples 8.1-3 are a good review of calculations (use reference tables and note y = 0 case)

## 8.3: The Loss Elimination Ratio / Effect of Inflation for Deductible

<div class = "blue">

The loss elimination ratio (LER) is the ratio of the decrease in the expected payment with an ordinary deductive to the expected payment without the deductible:

$$
  \textbf{Loss Elimination Ratio: } \quad \frac{\text{E}(X)-[\text{E}(X)-\text{E}(X \wedge d)]}{\text{E}(X)} = \frac{\text{E}(X \wedge d)}{\text{E}(X)}
$$
</div> <br>

<div class = "red">

For an ordinary deductible of $d$ after uniform inflation of $1+r$, the expected cost per loss is:

$$
  \textbf{Inflation-adjusted Expected Cost Per Loss: } \quad (1+r)\left\{ \text{E}(X) - \text{E}\left[X \wedge \frac{d}{1+r}\right] \right\}  
$$

</div>


## 8.4: Policy Limits

- The opposite of a deductible is a ***policy limit***

- Typically arises in a contract where for losses below $u$, the insurance pays the full loss, but for losses above $u$, the insurance pays only $u$

<div class = "green">

The policy limit produces a right-censored random variable, $Y$:

$$
  \begin{align}
  F_Y(y) &= \begin{cases}
             F_X(y), & y<u \\
             1, & y \geq u
           \end{cases} \\
  f_Y(y) &= \begin{cases}
            f_X(y), & y<u \\
            1-F_X(u), & y=u
          \end{cases}
  \end{align}
$$

</div><br>

<div class = "purple">

For a policy limit of $u$, after a uniform inflation of $1+r$:

$$
  \textbf{Inflation-adjusted Expected Cost with Policy Limit: } \quad (1+r) \ E\left[X \wedge \dfrac{u}{1+r}\right]
$$

</div>


## 8.5: Coinsurance, Deductibles, and Limits

- In the case of ***coinsurance***, the insurance company pays a proportion, $\alpha$, of the loss and the policyholder pays the remaining fraction. The loss variable, $X$, is transformed into $Y = \alpha X$

<div class = "blue">

If ordinary deductible, limit, coinsurance, and inflation are present, the per-loss random variable becomes:

$$
  Y^L = \begin{cases}
          0, & X < \dfrac{d}{1+r} \\
          a[(1+r)X-d], & \dfrac{d}{1+r} \leq X < \dfrac{u}{1+r} \\
          \alpha(u-d), & X \geq \dfrac{u}{1+r}
        \end{cases}
$$
</div> <br>

- The loss above which no additional benefits are paid is called the ***maximum covered loss*** (in the case above, it's $u$)

<div class = "blue">

This above-defined combined-coverage random variable has the following expected value in both the per-loss case and the per-payment case:

$$
  \begin{align}
  \text{E}(Y^L) &= \alpha(1+r) \left[ \text{E}\left(X \wedge \frac{u}{1+r}\right) - \text{E}\left(X \wedge \frac{d}{1+r}\right)   \right] \\
  \text{E}(Y^P) &= \frac{\text{E}\left(Y^L\right)}{1-F_X \left( \dfrac{d}{1+r} \right)} \\ \\
  \text{E}\left[ (Y^L)^2 \right] &= \alpha^2(1+r)^2 \{ \text{E}\left[(X \wedge u^*)^2\right] - \text{E}\left[(X \wedge d^*)\right]\ - 2d^* \text{E}(X \wedge u^*) + 2d^* \text{E}(X \wedge d^*) \} \\ \\
  &\textit{where } u^* = u/(1+r) \textit{ and } d^* = d/(1+r) 
  \end{align} 
$$

- These formulas can be used if some or all of the insurance items are employed (e.g., if there's no inflation, $r=0$; no coinsurance means $\alpha=1$)

</div>


## 8.6: The Impact of Deductibles on Claim Frequency

- Let $X_j$ represent the ***severity***, the ground-up loss on the $j^{th}$ such loss with no coverage modifications

- Let $N^L$ denote the number of losses

- Let $\nu = \Pr(X>d)$, where $d$ is the new deductible

- Let $I_j$ be an indicator variable: $I_j = 1$ if the $j^{th}$ loss results in a payment, and $I_j =0$ otherwise

- Then, $I_j$ has a Bernoulli distribution with parameter $\nu$ and pgf $P_{I_j}(z)=1-\nu+\nu z$ 

- Let $N^P = \sum_\limits{i=0}^{N^L} I_i$ represent the number of payments

<div class = "red">

The pgf of the number of payments distribution takes the following form:

$$
  P_{N^P}(z) =P_{N^L} \left[ P_{I_j}(z) \right] = P_{N^L}[1+\nu(z-1)]
$$
Special case: If there exists a $B(z)$ which is functionally independent of $\theta$, then:

$$
  \begin{align}
    P_{N^P}(z) = & P_{N^L}(z;\theta) = B[\theta(1-z)] \\
               = & B[\nu \theta (1-z)] \\
               = & P_{N^L}(z;\nu \theta)
  \end{align}
$$
(i.e., $N^P$ will have the same type of distribution as $N^L$, with a modified parameter)


</div>


# 9: Aggregate Loss Models

- The goal is to build a model for the total payments by an insurance system

- Could lump everything together and study the distribution of the sum, but a more accurate and flexible model can be constructed by examining frequency and severity separately

- So, just sum over $N$ individual payment amounts. The questions is: should each be considered a unique random variable, or can we model the payments collectively?

<div class = "green">

The ***collective risk model*** assumes all $X_j\text{s}$ to be independent and identically distributed (i.i.d.) with the following independence assumptions:

1. Conditional on $N=n$, the random variables $X_1,X_2, \dots, X_n$ are i.i.d. random variables

2. Conditional on $N=n$, the common distribution of the random variables, $X_1,X_2, \dots, X_n$ does not depend on $n$

3. The distribution of $N$ does not depend in any way on the values of $X_1, X_2, \dots, X_n$

</div> <br>

<div class = "purple">

The ***individual risk model*** represents the aggregate loss as a sum, $S = X_1 + \dots + X_n$, of a fixed number, $n$, of insurance contracts. The loss amounts for the contracts, ($X_1, X_2, \dots, X_n$), are assumed to be independent but are not assumed to be identically distributed 

- Special case of the collective risk model

</div>

<br>

<div class = "blue">

<center>

![](Figures/8+.png)
</center>


</div> <br>
 

#### Two strategies:

- $N^L$ is the number of accidents, including those that do not exceed the deductible, with loss variables $Y^L$

- $N^P$ is the number of payments, with loss variables $Y^P$


## 9.2: Model Choices

- It is useful for the severity distribution to be a scale distribution, because the choice of currency should not affect the result

- A similar consideration applies to the frequency distribution

- The concept of invariance over time suggests using frequency distributions that are ***infinitely divisible***


## 9.3: The Compound Model for Aggregate Claims

<div class = "red">

Approach:

1. Develop a model for the distribution of $N$ based on data

2. Develop a model for the common distribution of $X_j\text{s}$ based on data

3. Using these two models, carry out calculations to obtain the distribution of $S$

Assume 1+2 are complete (other chapters). What follows in this section is how to achieve 3.

</div>

### 9.3.1: Probabilities and Moments (<font color = "red">review</font>)

<div class = "green">

$$
  S = X_1 + X_2 + \dots + X_N \ \ \text{has distribution:} \\
$$

$$
  \begin{align}
    F_S(x) &= \Pr(S \leq x) \\
           &= \sum_{n=0}^\infty p_n F_X^{*n} (x)
  \end{align}
$$

- $F_X^{*n}$ is the "n-fold convolution" of the cdf of $X$

</div> <br>

<div class = "green">

$$
  F_X^{*0} = \begin{cases}
                0, & x<0, \\
                1, & x \geq 0
             \end{cases} 
$$
$$
  F_X^{*k} = \int_{-\infty}^\infty F_X^{*(k-1)} (x-y) \ dF_X(y), \quad \text{for } k=1,2,\dots
$$

</div> <br>

<div class = "green">

If $X$ is a continuous random variable with probability zero on non-positive values, we get:

$$
  \begin{align}
  F_X^{*k}(x) &= \int_0^x F_X^{*(k-1)} \ (x-y) \ f_X(y) \ dy, \quad  k = 2,3, \dots \\
  F_X^{*k}(x) &= \sum_{y=0}^x F_X^{*(k-1)} \ (x-y) \ f_X(y), \quad  x=0,1,\dots, \quad k=2,3,\dots
  \end{align}
$$

- Hence, $F_X^{*1} = F_X(x)$

</div>
<br>

<div class = "purple">


Differentiating yields:

$$
  \begin{align}
  f_X^{*k}(x) &= \int_0^x f_X^{*(k-1)} \ (x-y) \ f_X(y) \ dy \quad  k=2,3,\dots \\
  f_X^{*k}(x) &= \sum_{y=0}^x f_X^{*(k-1)} \ (x-y) \ f_X(y), \quad  x=0,1,\dots, \quad k=2,3,\dots
  \end{align}
$$

$$
  f_S(x) = \sum_{n=1}^\infty \  p_n \ f_X^{*n}(x) \quad \text{for } x>0 \\
  \Pr(S=0) = p_0 \text{ at } x=0
$$

</div>

<br>

<div class = "blue">

$$
  \begin{align}
  P_S(z) &= \text{E}[z^S] = P_N[P_X(z)] \\ 
  M_S(z) &= P_N[M_X(z)]
  \end{align}
$$
</div>


<br>

<div class = "blue">

$$
  \begin{align}
    \text{E}(S) &= \mu_{S1}' = \text{E}(N)\text{E}(X) \\
    \text{Var}(S) &= \mu_{S2} = \text{E}(N) \text{Var}(X) + \text{Var}(N)[\text{E}(X)]^2 \\
  \end{align}
$$

</div>

### 9.3.2: Stop-Loss Insurance

- When losses occur to a policyholder, it is called ***insurance coverage***

- When losses occur to an insurance company, it is called ***reinsurance coverage***

<div class = "red">

Insurance on aggregate losses, subject to a deductible, is called ***stop-loss insurance***. The expected cost of this insurance is called the ***net stop-loss premium*** and can be computed as $\text{E}[(S-d)_+]$, where $d$ is the deductible and the notation $(\cdot)_+$ means to use the value in parentheses if it is positive and to use zero otherwise.

$$
  \begin{align}
    \textbf{Net Stop-Loss Premium: } \quad \text{E}[(S-d)_+] &=  \int_d^\infty[1-F_S(x)] \ dx \\
                              &= \int_d^\infty (x-d) \ f_S(x) \ dx \\
                              &=  \sum_{x>d}   (x-d)  f_S(x)                               
  \end{align}
$$

</div>
<br>

<div class = "red">

Suppose that $\Pr(a<S<b)=0$. Then, for $a \leq d \leq b$,

$$
  \text{E}[(S-d)_+] = \dfrac{b-d}{b-a} \text{E}[(S-a)_+]+ \dfrac{d-a}{b-a} \text{E}[(S-b)_+]  
$$

- See Example 9.6 for usage (includes additional Theorem 9.5 and Corollary 9.6)

</div>


### 9.3.3: The Tweedie Distribution

- Member of the linear exponential family

- For certain parameter values, it is a compound Poisson distribution with gamma severity distribution

<div class = "green">

Let $N$ have a Poisson distribution with mean $\lambda$ and let $X$ have a gamma distribution with parameters $\alpha$ and $\gamma$. Then, the compound distribution $S = X_1 + X_2 + \dots + X_N$ has the following characteristics:

$$
  \begin{align}
    \Pr(S=0) &= \Pr(N=0) = e^{-\lambda}, \\
    f_S(s) &= \sum_{n=1}^\infty e^{-\lambda} \dfrac{\lambda^n}{n!} \dfrac{s^{n\alpha-1}e^{-s/\gamma}}{\Gamma(n\alpha) \gamma^{n\alpha}}, \\
    \text{E}(S) &= \text{E}(N)\text{E}(X) = \lambda\alpha\gamma, \\
    \text{Var}(S) &= \text{E}(N)\text{Var}(X) + \text{Var}(N)[\text{E}(X)]^2=\lambda\alpha\gamma^2+\lambda(\alpha\gamma)^2\alpha(\alpha+1)
  \end{align}
$$
</div> <br>

<div class = "green">

When re-parameterized through the following relations:

$$
  \lambda = \dfrac{\mu^{2-p}}{\phi(2-p)}, \quad \alpha = \dfrac{2-p}{p-1}, \quad \text{and} \quad \gamma = \phi(p-1)\mu^{p-1},  
$$
where $1<p<2, \text{ } \mu>0, \text{ and } \phi>0$, we get:

$$
  \text{E}(S) = \mu \quad \text{and} \quad \text{Var}(S) = \phi \mu^p
$$

- $\phi$ is called the ***dispersion parameter*** and allows for additional flexibility with respect to how the variance relates to the mean 

- When $\phi>1$, the distribution is called the ***overdispersed Poisson*** distribution

</div>




## 9.4: Analytic Results

<div class = "purple">

Suppose that $S_j$ has a compound Poisson distribution with Poisson parameter $\lambda_j$ and a severity distribution with cdf $F_j(x)$ for $j=1,2,\dotsm,n$. Suppose also that $S_1, S_2, \dots,S_n$ are independent. Then, $S = S_1 + \dots + S_n$ has a compound Poisson distribution with Poisson parameter $\lambda = \lambda_1 + \dots + \lambda_n$ and a severity distribution with cdf:

$$
  F(x) = \sum_{j=1}^n \dfrac{\lambda_j}{\lambda} F_j(x)
$$

- Example 9.9 is proper

</div>

## 9.5: Computing the Aggregate Claims Distribution

- Recall the compound distribution function: $F_S(x) = \sum_\limits{n=0}^\infty p_n F_X^{*n}$. It's not easy to evaluate, so we use methods to estimate it

- One approach is to use an ***approximating distribution***. Easy to use, but precision questionable

- The second approach is to evaluate the distribution is ***direct calculation***

- However, the ***recursive method*** can massively reduce calculations compared to direct calculation


## 9.6: The Recursive Method

<div class = "blue">

Let $f_X(x)$ be the severity distribution defined on $x = 0,1,2, \dots, m$. Then, if the frequency distribution, $p_k$ is a member of the $(a,b,1)$ or $(a,b,0)$ class, we can calculate $f_S(x)$ as follows:

$$
  \begin{align}
  \textbf{(a,b,1) Class: } \quad f_S(x) &= \frac{[p_1 - (a+b)p_0] f_X(x) + \sum_{y=1}^{x\wedge m} (a+by/x) f_X(y) f_S(x-y) }{1-a \ f_X(0)} \\
  &= p_1 f_X(x) + \int_0^x \left( a + \dfrac{by}{x}  \right) f_X(y) f_S(x-y) \ dy  \quad \text{(if severity continuous)}\\
  \textbf{(a,b,0) Class: } \quad f_S(x) &= \frac{\sum_{y=1}^{x\wedge m} (a+by/x) f_X(y) f_S(x-y) }{1-a \ f_X(0)} \\
  &= \dfrac{\lambda}{x} \sum_{y=1}^{x \wedge m} y f_X(y) f_S(x-y), \quad x = 1,2,\dots \quad \quad \quad \ \ \text{(if Poisson distribution)} 
  \end{align}
$$

</div>

- Sometimes probabilities are very small and cause recursion to fail. We can employ different strategies to counteract this ***underflow***

- Recursive formulas require really precise computation of values, because errors compound on each successive iteration of the formula

### 9.6.5: Constructing Arithmetic Distributions

- Sometimes it might be necessary to convert a continuous distribution into a discrete distribution (perhaps to simplify calculations)

- An ***arithmetic*** distribution is defined on the non-negative integers

- To arithmetize a distribution, it is important to preserve the properties of the distribution

#### 9.6.5.1: The Method of Rounding (Mass Dispersal)

<div class = "red">

Let $f_j$ denote the probability placed at $jh, j=0,1,2,\dots$. Then, set:

$$
  \begin{align}
  f_0 &= \Pr\left( X < \frac{h}{2} \right) = F_X \left( \frac{h}{2}-0    \right) \\
  f_1 &= \Pr \left( jh - \frac{h}{2} \leq X < jh + \frac{h}{2} \right) \\
      &= F_X \left( jh + \frac{h}{2} - 0 \right) - F_X \left( jh - \frac{h}{2} - 0 \right), \quad j = 1,2,\dots
  \end{align}  
$$

</div>

#### 9.6.5.2: The Method of Local Moment Matching

<div class = "red">

Constructs an arithmetic distribution that matches $p$ moments of the arithmetic and the true severity distributions. The system of $p+1$ equations is:

$$
  \sum_{j=0}^p (x_k + jh)^r m_j^k = \int_{x_k-0}^{x_k+ph-0} x^r \ dF_X(x), \quad r=0,1,2,\dots,p
$$

</div>

<center>

![](Figures/9.png)

</center>



## 9.7: Policy Modifications' Impact on Aggregate Payments

- We continue to assume that the presence of policy modifications does not have an underwriting impact on the individual loss distribution through an effect on the risk characteristics of the insured population (i.e., the ***ground-up*** distribution of the individual loss amount $X$ is assumed to be unaffected by the policy modifications, and only the payments themselves are affected)

- Consequently, any analysis of the aggregate payments, $S$, may be done on either a per-loss basis or on a per-payment basis (though it is more convenient to use per-loss)

- If the (approximated) distribution of $S$ is of more interest than the moments, then a per-payment basis is normally preferred

<div class = "green">

Assuming that an individual loss results in a payment with probability $\nu$:

$$
  \begin{align}
    F_{Y^L}(y) &= (1-\nu) + \nu F_{Y^P}, \quad y \geq 0 \\
    M_{Y^L}(z) &= (1-\nu) + \nu M_{Y^P} \\ \\
    P_{N^P}(z) &= P_{N^L}(1-\nu + \nu z) \\ \\
    M_S(z)     &= P_{N^L} [M_{Y^L}(z)] \\
               &= P_{N^P}[M_{Y^P}(z)]   
  \end{align}
$$

</div> <br>

- Example 9.14 is a good review of concepts!


## 9.8: The Individual Risk Model

- The ***individual risk model*** represents the aggregate loss as a fixed sum of independent (but not necessarily identically distributed) random variables: $S = \sum X_i$

- Generalization of the model: $X_j = I_j B_j$, where $I_1,\dots,I_n,B_1,\dots,B_n$ are all independent. $I_j$ is an indicator variable that takes on the value of $1$ with a probability $q_j$ and the value of 0 otherwise. $B_j$ is degenerate, with all probability on the value $b_j$

<div class = "purple">

In the context of life insurance, assume the following: the probability of death within a year is $q_j$ and the fixed benefit paid for the death of the $j^{th}$ person is $b_j$. Then, we have:

$$
  \begin{align}
  f_{X_j}(x) &= \begin{cases}
                  1-q_j, & x = 0, \\
                  q_j, & x = b_j
               \end{cases}  \\ \\
    \text{E}(S) &= \sum_{j=1}^n b_j q_j \\
    \text{Var}(S) &= \sum_{j=1}^n b_j^2 q_j (1-q_j) \\
    P_S(z) &= \prod_{j=1}^n (1-q_j + q_j z^{b_j}) \\ 
           &= [1 + q(z-1)]^n \quad \text{(identical risks)} \\
    M_S(z) &= \prod_{j=1}^n [1 - q_j + q_j M_{B_j}(z)] 
  \end{align}
$$

</div> <br>

<div class = "purple">

Letting $\mu_j = \text{E}(B_j)$ and $\sigma^2_j = \text{Var}(B_j)$, we get:

$$
  \begin{align}
  \text{E}(S) &= \sum_{j=1}^n q_j \mu_j \\
  \text{Var}(S) &= \sum_{j=1}^n [\ q_j \sigma_j^2 + q_j(1-q_j)\mu_j^2 ]
  \end{align}
$$

- Example 9.15 is a good one!

</div>

### 9.8.2: Parametric Approximation

<div class = "blue">

Find $\text{E}(S)$ and $\text{Var}(S)$ as above, then use a normal or log-normal distribution with that mean and variance to estimate the distribution

- Example 9.16 is a good one!


</div>



# 11: Maximum Likelihood Estimate

- Estimating heavy-tail distributions require a lot of information, whereas, in the normal case, the sample mean and variance are sufficient

- Set up an objective function, and then determine the parameter values that optimize the function

<div class = "red">

Let the data set be the $n$ events: $A_1,\dots,A_n$. Assume that the event $A_j$ results from observing the random variable $X_j$. The random variables $X_1,\dots,X_n$ need not have the same probability distribution, but their distributions must depend on the same parameter vector, $\theta$. The random variables are assumed to be independent.

$$
  \begin{align}
    \textbf{Likelihood Function: } \quad L(\theta) &= \prod_{j=1}^n \Pr(X_j \in A_j | \theta) \\
                                                  &= \prod_{j=1}^n f_{X_j}(x_j|\theta) \quad \ \quad \quad \quad \quad \quad \quad \text{(individual data)}  \\
                                                  &= \prod_{j=1}^k [F(c_j|\theta) - F(c_{j-1}|\theta)]^{n_j},  \ \quad \text{(group data)} \\
                                                  &= \prod_{j=1}^n\dfrac{\text{*from above*}}{1-F(d)}, \quad \quad \quad \quad \  \text{(if truncated at } d)
  \end{align}
$$

- The ***maximum likelihood estimate*** of $\theta$ is the vector that maximizes the likelihood function

- Care must be taken when maximizing this function because there may be local maxima in addition to the global maximum

$$
  \textbf{Loglikelihood Function: } \quad l(\theta) = \ln L(\theta)
$$

- To maximize $l(\theta)$ is the same thing as -- and is often easier than -- maximizing $L(\theta)$ 

- Examples 11.1-2 are good!

- ***Censored from above*** (right-censored) at $u$ means the variable's value doesn't change when it is less than $u$; however, the variable's value equals $u$ whenever the observed value is above $u$ (short put graph). Not additional complication regarding maximum likelihood.

- ***Truncated from below*** (left-truncated) at $d$ means the variable is not recorded when its observed value is less than $d$; otherwise, it is equal to its normal value. How to proceed regarding maximizing likelihood? Don't consider values lower than $d$, then subtract $d$ from other values (this is called ***shifting***)

- Example 11.5-6 are good!


</div> <br>


## 11.5: Variance and Interval Estimation for MLE


#### Finding Variance of Estimators

<div class = "green">

We want to be able to assess how good our estimators are for each parameter. To do that, we need a way to determine the variance of the parameter estimates. The process requires a few steps:

1. Consider $L(p_1,p_2,\dots, p_k)$ where $p_i$ represent the $k$ parameters of the distribution 

2. Take the natural log, yielding $l(p_1,p_2,\dots,p_k)$

3. Find $\dfrac{\partial^2l}{\partial p_j^2}$ for each parameter

4. Evaluate $\text{E}\left( \frac{\partial^2l}{\partial p_j^2} \right)$ for each parameter, keeping in mind specific characteristics of the underlying distribution (e.g., $\ln X_j$ has a normal distribution with mean $\mu$ and standard deviation $\sigma$ for a lognormal distribution)

5. Then, for each parameter, $p_j$, we have: $\widehat{\text{Var}}(\hat{p_j}) = \left[-\text{E}\left( \frac{\partial^2l}{\partial p_j^2} \right) \right]^{-1} = [I(\theta)]^{-1}$ (where we consider $I(\theta)$ to be the ***(Fisher) information***)


</div>

#### Finding Confidence Intervals for the True Parameter Values

<div class = "purple">

Once we have parameter estimates and variance estimates of the parameters, we can create confidence intervals for each of the parameters. This can be done as follows:

1. Estimate the parameters by finding the value for $p_j$ that solves the equation $\dfrac{\partial l}{\partial p_j} = 0$. We call this parameter estimate $\hat{p_j}$ 

2. We've already determined variance estimates above: $\widehat{\text{Var}}(\hat{p_j}) = \left[-\text{E}\left( \frac{\partial^2l}{\partial p_j^2} \right) \right]^{-1}$ 

3. For a given confidence level, we can construct confidence intervals as usual (e.g., 95% c.i. is $\pm 1.96 \sqrt{\text{Var}}$)

</div> <br>

- ***Example 11.7 is crucial!!*** 

- An alternative approach would be to use the observed data points (***observed information***) rather than using the more complicated expectations to find $\widehat{\text{Var}}(\hat{p_j})$


## 11.6: Functions of Asymptotically Normal Estimators

- Let $g$ be a function of a distribution's parameters. Can we construct a confidence interval for $g(p_1,p_2,\dots,p_k)$? Yes! 

<div class = "blue">

Let's assume $g$ is normally distributed. A construction of the $g$'s confidence interval proceeds as follows: 

1. An estimate of $g$, call is $\hat{g}$. This can be calculated using our previously determined parameter estimates, $\hat{p_i}$

2. An estimate of the variance of $g$ is more difficult, but we can use the ***delta method***. For two or less parameters, we have: 

$$
  \widehat{\text{Var}}[g(\hat{p_1}, \hat{p_2} )] = 
  \begin{bmatrix} 
    \widehat{\dfrac{\partial g}{\partial p_1}} & \widehat{\dfrac{\partial g}{\partial p_2}}   
  \end{bmatrix} 
  \begin{bmatrix}
    \left[-\text{E}\left(\dfrac{\partial^2 l}{\partial p_1^2}\right)\right]^{-1} & \left[-\text{E}\left(\dfrac{\partial^2 l}{\partial p_1 \partial p_2}\right)\right]^{-1} \\
    \left[-\text{E}\left(\dfrac{\partial^2 l}{\partial p_1 \partial p_2}\right)\right]^{-1} & \left[-\text{E}\left(\dfrac{\partial^2 l}{\partial p_2^2}\right)\right]^{-1}
  \end{bmatrix} 
  \begin{bmatrix} 
    \widehat{\dfrac{\partial g}{\partial p_1}} \\
    \widehat{\dfrac{\partial g}{\partial p_2}}
  \end{bmatrix}
$$

3. Construct a confidence interval as usual, with $g = \hat{g} \pm Z_\alpha \sqrt{\widehat{\text{Var}}(g)}$

</div><br>

- ***Examples 11.10-11 are crucial!***


## 11.7: Non-normal Confidence Intervals

- In the above section, we assume that the estimators are normally distributed. This is true in the extreme case where $n \to \infty$, but is not true, in general.

<div class = "red">

We can get a more precise confidence interval by solving the following inequality for $\theta$:

$$
  l(\theta) \geq  l(\hat{\theta}) - 0.5 \chi_\alpha^2, \\ \textit{where } \chi_\alpha^2 \textit{ is the } (1-\alpha) \textit{ percentile from the chi-square distribution}
$$

</div> <br>

- Example 11.12 is a good one!



# 12: Frequentist Estimation for Discrete Distributions

There are special considerations that apply when estimating parameters for discrete distributions:

## 12.1: The Poisson Distribution

<div class = "green">

$$
  \begin{align}
    p_k &= \dfrac{e^{-\lambda}\lambda^k}{k!} \\
    \ln p_k &= -\lambda + k \ln \lambda - \ln k! \\ \\
    L &= \prod_{k=0}^\infty p_k^{n_k} \\ 
    l &=  \sum_{k=0}^\infty n_k \ln p_k \\ \\
  \end{align}
$$
</div> <br>

<div class = "green">

$$
  \begin{align}
    \hat{\lambda} &= \bar{x} \\
    \text{E}(\hat{\lambda}) &= \lambda \\
    \text{Var}(\hat{\lambda}) &= \frac{\lambda}{n}
  \end{align}
$$

</div><br>

#### Contribution to Likelihood in Special Cases 

<div class = "green">

- If the final entry is $k+$, representing $k$ or more claims were observed $n_{k+}$ times, the contribution to the likelihood function is: $(p_k + p_{k+1} + \dots)^{n_{k+}} = (1-p_0 - \dots - p_{k-1})^{n_{k+}}$ 

- Suppose there were five observations at frequencies $3-5$. The contribution to the likelihood function would be: $(p_3 + p_4 + p_5)^5$

</div>




## 12.2: The Negative Binomial Distribution

<div class = "purple">

$$
  \begin{align}
    l &= \sum_{k=0}^\infty n_k \ln p_k \\
      &= n_k \left[ \ln {r+k-1 \choose k} - r \ln(1+\beta) + k \ln \beta - k \ln (1+\beta) \right]
  \end{align}
$$
</div> <br>

<div class = "purple">

$$
  \begin{align}
    \hat{\mu} &= \hat{r}\hat{\beta} = \dfrac{\sum_{k=0}^\infty k n_k} {n} = \bar{x} \\ \\
    \hat{\beta} &= \dfrac{\hat{\mu}}{\hat{r}} = \dfrac{\bar{x}}{\hat{r}} \\ \\
    \hat{r} &= \text{solve}\left(n \ln \left(1+ \dfrac{\bar{x}}{\hat{r}}\right) - \sum_{k=1}^\infty n_k     \left( \sum_{m=0}^{k-1} \dfrac{1}{\hat{r} + m} \right)=0, \hat{r}\right)
  \end{align}
$$

</div>

## 12.3: The Binomial Distribution

<div class = "blue">

$$
  \begin{align}
    l &= \sum_{k=0}^m n_k \ln p_k \\
      &= \sum_{k=0}^m n_k \left[ \ln {m \choose k} + k \ln q + (m-k) \ln (1-q)  \right] \\ \\
    \hat{q} &= \dfrac{1}{\hat{m}} \dfrac{\sum_{k=0}^\infty k n_k}{\sum_{k=0}^\infty n_k} = \dfrac{\text{# of observed events}}{\text{maximum # of possible events}} 
  \end{align}
$$
</div><br>


#### Estimating m Using a Likelihood Profile

<div class = "blue">

1. Start with $\hat{m}$ equal to the largest observation

2. Obtain $\hat{q}$ using the equation above

3. Calculate the log-likelihood of these values

4. Increase $\hat{m}$ by 1

5. Repeat steps 2-4 until a maximum is found

<center>

![](Figures/14+.png)

</center>

</div>

## 12.4: The (a,b,1) Class

<div class = "red">

$$
  \begin{align}
    L &= \left( p_o^M \right)^{n_0} \prod_{k=1}^\infty \left( p_k^M \right)^{n_k} = \left( p_0^M \right)^{n_0} \prod_{k=1}^\infty \left[ (1-p_0^M) p_k^T \right]^{n_k} \\ \\
    l &= n_0 \ln p_0^M + \sum_{k=1}^\infty n_k \left[\ln (1-p_0^M) + \ln p_k^T\right] \\
      &= n_0 \ln p_0^M + \sum_{k=1}^\infty n_k \ln (1-p_0^M) + \sum_{k=1}^\infty n_k \left[ \ln p_k - \ln (1-p_0) \right]
  \end{align}
$$
</div><br>

<div class = "red">

We can construct zero-modified models using the appropriate zero-truncated subclass $(a,b,1)$ distributions (p. 14-16 in reference tables) by setting: 

$$
  \hat{p}_0^M = \dfrac{n_0}{n}
$$
</div><br>


<center>

![](Figures/15+.png)

</center>


## 12.6: The Effect of Exposure on Maximum Likelihood Estimation

- If there are multiple exposures ($e_k$) for a given event, we need to multiply the parameters by $e_k$ in likelihood function, which changes estimators

- This occurs when, for instance, a variable is censored (anything over a value set to max); we need to multiply the max by the number of exposures (number of values greater than or equal to the max set by the censoring)

- ***Example 12.9*** is a really good one!








# 13: Bayesian Estimation (<font color = "red">review</font>)

- All of the previous discussions on estimation have  assumed a frequentist approach: the population distribution is fixed (but unknown) and our decisions have been concerned with the sample and other subsets of the population.

- The Bayesian approach assumes only sample data are relevant and the population distribution is variable


## 13.1: Definitions and Bayes' Theorem

<div class = "green">

- The ***prior distribution*** is a probability distribution over the space of possible parameter values. We denote it $\pi(\theta)$ and it *represents our opinion concerning the relative chances that various values of $\theta$ are the true value of the parameter*. It's basically a set of weights depending on what we believe should be the case (our prior knowledge)

- An ***improper prior distribution*** is one for which the probabilities are non-negative, but their sum (or integral) is infinite

- The ***model distribution*** is the regular probability distribution for a particular value of the parameter. We denote this: $f_{\vect{X}|\Theta}(\vect{x}|\theta)$

- The ***joint distribution*** is the model distribution with our assumptions (the prior) as a weight denoted as follows: $f_{\vect{X},\Theta}(\vect{x}|\theta)=f_{\vect{X}|\Theta}(\vect{x}|\theta) \cdot \pi(\theta)$

- The ***marginal distribution of $\vect{x}$*** is the  integral of the joint distribution: $f_{\vect{X}}(\vect{x}) = \int f_{\vect{X}|\Theta} \cdot \pi(\theta) \ d\theta$

- The ***posterior distribution*** is the conditional probability distribution of the parameters given the observed data: 

$$
  \pi_{\Theta|\vect{X}}(\theta|\vect{x}) = \dfrac{\text{joint}}{\text{marginal}} = \dfrac{f_{\vect{X}|\Theta}(\vect{x}|\theta) \cdot \pi(\theta)}{\int f_{\vect{X}|\Theta} \cdot \pi(\theta) \ d\theta}
$$

- The ***predictive distribution*** is the conditional probability distribution of a new observation $y$ given the data $\vect{x}$. Let $f_{Y|\Theta}(y|\theta)$ be the pdf of the new observation given the parameter value. Then, the predictive distribution is defined as follows:

$$
  f_{Y|\vect{X}}(y|\vect{x}) = \int f_{Y|\Theta}(y|\theta) \cdot \pi_{\Theta|\vect{X}}(\theta|\vect{x}) \ d\theta
$$
</div><br>

## 13.2: Inference and Prediction

- So, we started with a distribution that quantifies our knowledge about the parameter and ended with a revised distribution. Good, but what if we could get a specific number as a result, rather than a distribution?

<div class = "purple">

A ***loss function $l_j(\hat{\theta}_j,\theta_j)$*** describes the penalty paid by the investigator when $\hat{\theta_j}$ is the estimate and $\theta_j$ is the true value of the $j^{th}$ parameter 


#### Common Loss Functions

1. ***Squared-error loss***: $l(\hat{\theta},\theta) = (\hat{\theta}-\theta)^2$ 

2. ***Absolute loss***: $l(\hat{\theta},\theta) = \lvert\hat{\theta}-\theta)\rvert$

3. ***Zero-one loss***: $l(\hat{\theta},\theta) = 0 \text{ if } \hat{\theta}=\theta \text{, and } 1 \text{ otherwise}$

</div><br>

<div class = "purple">

The ***Bayes estimate*** (of a a parameter) for a given loss function is the one that minimizes the expected loss given the posterior distribution of the parameter in question:

- Bayes estimate (squared-error loss - default!) = mean of the posterior distribution

- Bayes estimate (absolute loss) = median of the posterior distribution

- Bayes estimate (zero-one loss) = mode of the posterior distribution

</div> <br>

<div class = "blue">

$$
  \begin{align}
  \textbf{Expected Value of Predictive Distribution: } \quad \text{E}(Y|\vect{x}) &= \int \text{E}(Y|\theta) \cdot \pi_{\Theta|\vect{X}}(\theta|\vect{x}) \ d\theta \\
  &= \text{E}_{\Theta|\vect{x}}[\text{E}(Y|\Theta)]
  \end{align}
$$

- ***Examples 13.8-9*** might be useful (confusing!)

</div><br>

<div class = "red">

- The points $a<b$ define a $100(1-\alpha)\%$ ***credibility interval*** for $\theta_j$ provided that $\Pr(a \leq \Theta_j \leq b|\vect{x}) \geq 1-\alpha$

- If the posterior random variable $\theta_j|\vect{x}$ is continuous and uni-modal, then the credibility interval with the smallest width ($b-a$) is the unique solution to the following set of equations:

$$
  \begin{align}
    &1.\quad \int_a^b \pi_{\Theta_j|\vect{X}}(\theta_j|\vect{x}) \ d\theta_j = 1 - \alpha \\
    &2. \quad \pi_{\Theta|\vect{X}}(a|\vect{x}) = \pi_{\Theta|\vect{X}}(b|\vect{x})
  \end{align}
$$

- ***Example 13.5*** describes this (tricky!)

</div><br>

<div class = "red">

For any posterior distribution, the $100(1-\alpha)\%$ ***HPD credibility set*** is the set of parameter values $C$ such that:

$$
  \begin{align}
  &1. \quad\Pr(\theta_j \in C) \geq 1-\alpha \\ \\
  &2. \quad C = \{ \theta_j: \pi_{\Theta_j|\vect{X}}(\theta_j|\vect{x} \geq c \} \quad \text{for some } c, \\
  &\text{where } c \text{ is the largest value for which the inequality from 1. holds}
  \end{align}
$$

</div>


# 15: Model Selection

<div class = "green">

"All models are wrong, but some models are useful."

"Far better an approximate answer to the right question, which is often vague, than an exact answer to the wrong question, which can always be made precise."

</div>

## 15.2: Representations of the Data and Model

<div class = "purple">

To make a comparison to the empirical values, the model must be truncated. Let the truncation point in the data set be $t$. The modified functions are:

$$
  \begin{align}
  F^*(x) &= \begin{cases}
              0, & x<t, \\
              \dfrac{F(X) - F(t)}{1-F(t)}, & x \geq t
           \end{cases} \\ \\
  f^*(x) &= \begin{cases}
               0, & x<t, \\
               \dfrac{f(x)}{1-F(t)}, & x \geq t
            \end{cases}
  \end{align}
$$

- A subscript on a distribution or density function equal to the sample size indicates that it is an empirical model, while no adornment or the use of an asterisk (*) indicates the estimated parametric model


</div>


## 15.3: Graphical Comparison of the Density and Distribution Functions

<div class = "blue">

The most direct way to see how well the model and the data match is to plot the respective density and distribution functions. Letting the model distribution function be $F^*(x)$ and the empirical distribution be $F_n(x)$, we have:

<center>

![](Figures/10+.png)

</center>

</div><br>

<div class = "red">

Letting $F_n(x)$ be the empirical distribution with $n$ being the sample size, we can plot the difference between the empirical distribution and the model distribution, $F^*(x)$. We call this difference, $D(x) = F_n(x) - F^*(x)$:

<center>

![](Figures/11+.png)

</center>

</div><br>

<div class = "green">

Another way to highlight differences is the ***p-p plot*** or ***probability plot***. First, order the observation. Then, construct a plot with points $\left(F_n(x_j), F^*(x_j)\right)$:

<center>

![](Figures/12+.png)

</center>

</div>


## 15.4: Hypothesis Tests

- How can we go beyond just a graphical representation and determine more scientifically if our model is a good one? Hypothesis tests!

- The null hypothesis could be something like: $\quad \quad H_0 \text{: The data came from a population with a Binomial distribution with } q=0.5 \text{ and } m=2$

- Often times, the null hypothesis states the name of the model, but not the parameters

- When parameters are estimated from data, the tests become approximate, increasing the probability of Type II (model deemed acceptable, but it's really not acceptable) errors. To avoid this, it is useful to split the data into training and test sets

### 15.4.1: The Kolmogorov-Smirnov Test

- Let $t$ be the left truncation point ($t=0$ if there is no truncation) and let $u$ be the right censoring point ($u = \infty$ if there is no censoring). 

<div class = "purple">

The Kolmogorov-Smirnov test statistic, the maximum difference between the empirical distribution and the model distribution, is defined as:

$$
  \textbf{Kolmogorov-Smirnov test statistic: } \quad D = \max_{t \leq x \leq u} \lvert F_n(x) - F^*(x) \rvert
$$

$$
  \begin{gather}
    \alpha && \text{Critical Value} \\
    \hline 
    0.10   && 1.22/\sqrt{n} \\
    0.05   && 1.36/\sqrt{n} \\
    0.01   && 1.63/\sqrt{n}
  \end{gather}
$$

- If $D > \text{Critical Value}(\alpha)$, we reject the null hypothesis and say the model is inappropriate

</div>

### 15.4.3: The Chi-Square Goodness-of-Fit Test

<div class = "blue">

1. Select $k-1$ arbitrary values, $t=c_0 < c_1 < \dots < c_k = \infty$

2. Let $\hat{p_j} = F^*(c_j) - F^*(c_{j-1})$ be the probability a truncated observation falls in the interval from $c_{j-1}$ to $c_j$

3. Let $p_{nj} = F_n(c_j) - F_n(c_{j-1})$ be the same for the empirical distribution


$$
  \begin{align}
    \textbf{Chi-Square test statisitic: } \quad \chi^2 &= \sum_{j=1}^k \dfrac{n\left(\hat{p}_j - p_{nj}\right)^2 }{\hat{p}_j} \\
           &= \sum_{j=1}^k \dfrac{\left( E_j - O_j\right)^2}{E_j}, \quad  E_j =n\hat{p}_j \quad O_j = np_{nj}
  \end{align}
$$

<center>

![](Figures/13+.png)

</center>

- The critical value comes from the chi-square distribution with degrees of freedom equal to the number of terms in the sum ($k$) minus 1 minus the number of estimated parameters

</div>

### 15.4.4: The Likelihood Ratio Test

- ***If distribution $A$ is a special cases of distribution $B$***, we can compare the likelihood that one distribution is a better fit than the other:

$$
  H_0: \text{The data came from a population with distribution } A \\
  H_1: \text{The data came from a population with distribution } B
$$

<div class = "red">

1. Let the likelihood function be written as $L(\theta)$

2. Let $\theta_0$ be the value of the parameters that maximizes $L$ under the assumptions of the null hypothesis

3. Let $\theta_1$ be the maximum likelihood estimator, where the parameter values are constrained by the assumptions of the alternative hypothesis 

$$
  \textbf{Likelihood Ratio test statistic: } \quad T = 2 \ln(L_1/L_0) = 2(\ln L_1 - \ln L_0) 
$$

- Reject the null hypothesis if $T>c$, where $c$ is calculated from $\alpha = \Pr(T>c)$, where $T$ has a chi-square distribution with degrees of freedom equal to the number of free parameters in the model from the alternative hypothesis less the number of free parameters in the model from the null hypothesis (e.g., $c = 3.841$ for $1 \text{ d.f}$ and $\alpha = 0.95$)

</div>

## 15.5: Selecting a Model

- The principle of ***parsimony*** states that unless there is considerable evidence to do otherwise, a simpler model is preferred

### 15.5.2: Judgment-Based Approaches

<div class = "green">

- Analyst's experience is critical

- It may be important to fit the tail well or match the mode, etc.

- The situation might completely determine the distribution (e.g., the scenario could represent exactly the binomial distribution, for instance)

</div>

### 15.5.3: Score-Based Approaches

<div class = "purple">

A more automated approach might be preferable. In this case, we need some score (or a set of scores) that will guide us in deciding the best model. Some options include:

1. The **Kolmogorov-Smirnov test statistic** (choose model with smallest value)

2. The **Chi-Square Goodness-of-Fit test statistic** (choose model with smallest value or highest $p$-value) [$p$-value = parsimonious!]

3. The **Likelihood function** (choose model with largest value)

4. The ***SBC/BIC***: $\ln L - \dfrac{r}{2} \ln n \quad$ [$r$ =  # of parameters, $n$= sample size] (choose largest value) 

5. The ***AIC***: $\ln L - r$ [$r$ = # of parameters] (choose largest value)

</div>



