---
title: "Implementing Black Scholes & Vasiciek Models."
subtitle: "A project implementing the Black-Scholes and Vasiciek models to determine the prices of call options and bonds respectively."
excerpt: "In this project I implement the Black-Scholes and Vasiciek models to determine the prices of call options and bonds respectively. "
date: 2020-11-30
author: "John Robin Inston"
draft: false
images:
series:
tags:
categories:
  - Quantitative Analysis
  - Stochastic Processes
  - Financial Mathematics
layout: single
links:
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/JohnRobinInston/project1_BSVasicek

---

### Abstract

In this project we investigate the properties of financial stochastic processes.  We first consider the implementation of the Black-Scholes model for determining the price of European call options on stock before then utlising the Vasicek model to calculate the future price of bonds.  We discuss the background, benefits and drawbacks of both models before conducting our investigation and summarising our findings.

<br>

![](featured-hex.png)

---

<br>

## 1.  Introduction

The Black-Scholes model is a mathematical model developed by the economists Fischer Black, Myron Scholes and Robert Merton which is widely used in the pricing of options contacts. The model assumes that the price of heavily traded assets follows a geometric Brownian motion with constant drift and volatility. When applied to a stock option, the model incorporates the constant price variation of the stock, the time value of money, the option’s strike price, and the time to the option’s expiry.
<br>

The Vasicek model is a mathematical model used in financial economics to estimate potential pathways for future interest rate changes. The model states that the movement of interest rates is affected only by random (stochastic) market movements and models interest rate movements as a factor composed of market risk, time, and equilibrium value - where the rate tends to revert towards the mean of those factors over time.
<br>

We will first investigate how the price of a European call option - as given by the Black- Scholes model - varies with changes in time, interest rates, strike price and volatility. We will then simulate an Ornstein-Uhlenbeck process and use this to simulate the price of a bond - using the Vasicek model - over a given time period before evaluating the distribution of the simulated prices.

<br>

---

<br>

## 2.  The Black-Scholes Model

The price at time <img src="https://render.githubusercontent.com/render/math?math=t_0=0"> of a European call option ('ECO') on a stock with strike price <img src="https://render.githubusercontent.com/render/math?math=c">, expiry time <img src="https://render.githubusercontent.com/render/math?math=t_0">, initial stock price <img src="https://render.githubusercontent.com/render/math?math=S_0">, interest rate <img src="https://render.githubusercontent.com/render/math?math=\rho"> and volatility <img src="https://render.githubusercontent.com/render/math?math=\sigma"> is given by the Black-Scholes ('BS') formula.  This is given below in Equation 1.

<br>

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large P_{t_0}=S_0\Phi\big(\frac{\log(S_0/c)%2B(\rho%2B\sigma^2/2)t_0}{\sigma\sqrt{t_0}}\big)-c\exp(\rho t_0)\Phi\big(\frac{\log(s_0/c)%2B(\rho-\sigma^2/2)t_0}{\sigma\sqrt{t_0}}\big)."></p>

<br>

We plot the price over time of the ECO <img src="https://render.githubusercontent.com/render/math?math=P_t"> for <img src="https://render.githubusercontent.com/render/math?math=0\leq t\leq 10"> with <img src="https://render.githubusercontent.com/render/math?math=s_0=1">, <img src="https://render.githubusercontent.com/render/math?math=\sigma^2=0.02">, <img src="https://render.githubusercontent.com/render/math?math=\rho=0.03"> and <img src="https://render.githubusercontent.com/render/math?math=c=1">.  This is shown below in Figure 1.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure1-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 1 - Option price <img src="https://render.githubusercontent.com/render/math?math=P_t"> over time <img src="https://render.githubusercontent.com/render/math?math=t">.</p>

<details>
<summary>R code for Figure 1.</summary>
<p>

```{c#}
library(ggplot2)
library(reshape)
library(tidyr)
library(gridExtra)

## 2.1 Price series plot

# Define initial values
S0 = 1 
sigma = sqrt(0.02)
rho = 0.03
c = 1
t = seq(0.001, 10, by=0.001)

# Define price
P = S0*pnorm((log(S0/c)+(rho+(sigma^2)/2)*t)/(sigma*sqrt(t)))-(c*exp(-rho*t))*pnorm((log(S0/c)+(rho-(sigma^2)/2)*t)/(sigma*sqrt(t)))

# Produce plot for figure 1
fig1 = data.frame(P, t)
plot1 = ggplot(data = fig1, aes(t,P))+geom_line() + 
  ggtitle("Option Price over time.") +
  xlab("Time (t)") + ylab("Option Price (P)")
plot1
```

</p>
</details> 

<br>

We see that the price <img src="https://render.githubusercontent.com/render/math?math=P_t"> is increasing over time, which makes sense as the values of the ECO should be dependent on the time the underlying stock has to increase in value.  From Equation 1 we can see that <img src="https://render.githubusercontent.com/render/math?math=P_t\rightarrow S_0"> as <img src="https://render.githubusercontent.com/render/math?math=t\rightarrow\infty">.

We plot the price <img src="https://render.githubusercontent.com/render/math?math=P_{10}"> at time <img src="https://render.githubusercontent.com/render/math?math=t=10"> as we vary each of <img src="https://render.githubusercontent.com/render/math?math=\sigma">, <img src="https://render.githubusercontent.com/render/math?math=\rho"> and <img src="https://render.githubusercontent.com/render/math?math=c"> in turn - shown in Figure 2, Figure 3 and Figure 4 respectively.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure2-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 2 - Option price <img src="https://render.githubusercontent.com/render/math?math=P_{10}"> at time <img src="https://render.githubusercontent.com/render/math?math=t=10"> for increasing volatility <img src="https://render.githubusercontent.com/render/math?math=\sigma">.</p>

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure3-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 3 - Option price <img src="https://render.githubusercontent.com/render/math?math=P_{10}"> at time <img src="https://render.githubusercontent.com/render/math?math=t=10"> for increasing interest rates <img src="https://render.githubusercontent.com/render/math?math=\rho">.</p>

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure4-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 4 - Ooption price <img src="https://render.githubusercontent.com/render/math?math=P_{10}"> at time <img src="https://render.githubusercontent.com/render/math?math=t=10"> for increasing strike price <img src="https://render.githubusercontent.com/render/math?math=c">.</p>

<br>

<details>
<summary>R code for Figures 2, 3 and 4.</summary>
<p>

```{c#}

## 2.2 Investigation

# Set t=10
t10=10

## 2.2.1 Varying volatility (sigma)

# Define varying sigma 
sigma1=seq(0,2, by=0.01)

# Calculate new price series
P1=S0*pnorm((log(S0/c)+(rho+(sigma1^2)/2)*t10)/(sigma1*sqrt(t10)))-(c*exp(-rho*t10))*pnorm((log(S0/c)+(rho-(sigma1^2)/2)*t10)/(sigma1*sqrt(t10)))

# Produce plot for figure 2
fig2 = data.frame(P1, sigma1)
plot2 = ggplot(data = fig2, aes(sigma1,P1))+geom_line() + 
  ggtitle("Option price at time t=10 for various volatility levels.") +
  xlab("Volatility (sigma)") + ylab("Option Price (P) at t=10.")

## 2.2.2 Varying interest rate (rho)

# Define varying rho
rho2=seq(0, 0.8, by=0.005) 

# Calculate new price series
P2=S0*pnorm((log(S0/c)+(rho2+(sigma^2)/2)*t10)/(sigma*sqrt(t10)))-(c*exp(-rho2*t10))*pnorm((log(S0/c)+(rho2-(sigma^2)/2)*t10)/(sigma*sqrt(t10)))

# Produce plot for figure 3
fig3 = data.frame(P2, rho2)
plot3 = ggplot(data = fig3, aes(rho2,P2))+geom_line() + 
  ggtitle("Option price at time t=10 for various interest rates.") +
  xlab("Interest Rate (rho)") + ylab("Option Price (P) at t=10.")

## 2.2.3 Varying strike price (c)

# Define varying c
c3=seq(0,5, by=0.01)

# Calculate new price series
P3=S0*pnorm((log(S0/c3)+(rho+(sigma^2)/2)*t10)/(sigma*sqrt(t10)))-(c3*exp(-rho*t10))*pnorm((log(S0/c3)+(rho-(sigma^2)/2)*t10)/(sigma*sqrt(t10)))

# Produce plot for figure 4
fig4 = data.frame(P3, c3)
plot4 = ggplot(data = fig4, aes(c3,P3))+geom_line() + 
  ggtitle("Option price at time t=10 for various strike prices.") +
  xlab("Strike Price (c)") + ylab("Option price (P) at t=10.")

# Combining figures 2, 3 and 4
grid.arrange(plot2, plot3, plot4, ncol=3)

```

</p>
</details> 

<br>

As both volatility <img src="https://render.githubusercontent.com/render/math?math=\sigma"> and interest rates <img src="https://render.githubusercontent.com/render/math?math=\rho"> increase we see that <img src="https://render.githubusercontent.com/render/math?math=P_{10}\rightarrow S_0">.  This is because high underlying stock volatility and interest rates both increase the potential option return which increases the options value up to the initial price of the stock.  The option value is limited to this value because if the option price were to rise above the stock price then there would be no reason for investors to purchase the option rather than the stock.

As the strike price <img src="https://render.githubusercontent.com/render/math?math=c"> increases we see that <img src="https://render.githubusercontent.com/render/math?math=P_{10}\rightarrow 0">.  This is because if the option has a high strike price it is less likely that the underlying stock will reach this price during the period of the option which makes the option less valuable.

<br>

---

<br>

## 3. Ornstein-Uhlenbeck Processes

The spot-rate <img src="https://render.githubusercontent.com/render/math?math=\{R_s:s>0\}"> is an Ornstein-Uhlenbeck (OU) process - with initial spot rate <img src="https://render.githubusercontent.com/render/math?math=R_0">, long-term mean <img src="https://render.githubusercontent.com/render/math?math=\mu"> and reversion speed <img src="https://render.githubusercontent.com/render/math?math=\theta>0"> - which is given by Equation 2 below.

<br>

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large R_s=e^{-\theta s}R_0%2B(1-e^{-\theta s})\mu%2B X_s."></p>

<br>

Here, <img src="https://render.githubusercontent.com/render/math?math=X_s"> is an OU process with volatility <img src="https://render.githubusercontent.com/render/math?math=\sigma>0"> and reversion parameter <img src="https://render.githubusercontent.com/render/math?math=\theta>0">. This is equivalent to stating the following.

<br>

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large E[X_s]=0 \hspace{.5cm}\text{and}\hspace{.5cm}Cov(X_s, X_t)=\frac{\sigma^2}{2\theta}e^{-\theta(s+t)}e^{2\theta\min(s, t)-1}."></p>

<br>

We simulate the OU process <img src="https://render.githubusercontent.com/render/math?math=\{R_s:0\leq s\leq 10\}"> for <img src="https://render.githubusercontent.com/render/math?math=R_0=0.1">, <img src="https://render.githubusercontent.com/render/math?math=\theta=0.5">, <img src="https://render.githubusercontent.com/render/math?math=\mu=0.05"> and <img src="https://render.githubusercontent.com/render/math?math=\sigma=0.02"> and produce a plot of our results.  These are shown below in Figure 5.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure5-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 5 - Spot rate <img src="https://render.githubusercontent.com/render/math?math=\{R_s:0\leq s\leq 10\}"> over time <img src="https://render.githubusercontent.com/render/math?math=s">.</p>

<br>

The expressions for the mean and variance of this process are given by:

<br>

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large E[R_s]=e^{-\theta s}R_0+(1+e^{-\theta s})\mu,"></p>

<br>

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large V[R_s]=\frac{\sigma^2}{2\theta}(1-e^{-s\theta s})."></p>

<br>

We plot the mean <img src="https://render.githubusercontent.com/render/math?math=E[R_s]"> and variance <img src="https://render.githubusercontent.com/render/math?math=V[R_s]"> against time in Figures 6 and 7 below.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure6-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 6 - Expected value <img src="https://render.githubusercontent.com/render/math?math=E[R_s]"> over time <img src="https://render.githubusercontent.com/render/math?math=s">.</p>

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure7-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 7 - Variance <img src="https://render.githubusercontent.com/render/math?math=V[R_s]"> over time <img src="https://render.githubusercontent.com/render/math?math=s">.</p>

<br>

From Figures 6 and 7 we evaluate what the mean and variance tend to as <img src="https://render.githubusercontent.com/render/math?math=s\rightarrow\infty">, for which we obtain the following expressions:

<br>

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large \lim_{(s\rightarrow\infty)}\{E[R_s]\}=\mu\hspace{.5cm}\text{and}\hspace{.5cm}\lim_{(s\rightarrow\infty)}\{V[R_s]\}\frac{\sigma^2}{2\theta}."></p>

<br>

We can see from Figure 6 that the expectation is tending towards the long-term mean <img src="https://render.githubusercontent.com/render/math?math=\mu=0.05"> as expected.  We can also see from Figure 7 that the variance is tending towards 0.04, which is as expected per our expression for the limit of <img src="https://render.githubusercontent.com/render/math?math=V[R_s]"> as <img src="https://render.githubusercontent.com/render/math?math=s\rightarrow\infty">.

<br>

If we change the value of <img src="https://render.githubusercontent.com/render/math?math=R_0"> we will change the starting point of the process.  If we change the value of <img src="https://render.githubusercontent.com/render/math?math=\mu"> we change the value which the expectation of the process will tend towards.  If we increase the value of the reversion parameter <img src="https://render.githubusercontent.com/render/math?math=\theta"> only, we will decrease the value that the variance of the process will tend towards.  Similarly, if we increase the value of the volatility <img src="https://render.githubusercontent.com/render/math?math=\sigma"> only, we will increase the value that the variance of the process will tend towards.

<br>

---

<br>

## 4. The Vasicek Model

The Vasicek model defines the price <img src="https://render.githubusercontent.com/render/math?math=Q_t "> at time 0 of a bond paying one unit at time <img src="https://render.githubusercontent.com/render/math?math=t "> as:

<p style="text-align: center;"> 
<img src="https://render.githubusercontent.com/render/math?math=\Large Q_t=\exp(-\int_0^t R_sds)"></p>

where <img src="https://render.githubusercontent.com/render/math?math=R_s "> is the OU process defined in Section 3.

<br>

We plot 10 simulations of the bond price <img src="https://render.githubusercontent.com/render/math?math=Q_t "> at time 0 of a bond paying one unit at time <img src="https://render.githubusercontent.com/render/math?math=t "> for <img src="https://render.githubusercontent.com/render/math?math=R_0=0.1 ">, <img src="https://render.githubusercontent.com/render/math?math=\theta=0.5 ">, <img src="https://render.githubusercontent.com/render/math?math=\mu=0.05 "> and <img src="https://render.githubusercontent.com/render/math?math=\sigma=0.02 "> - shown in Figure 8 below.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure8-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 8 - Bond price <img src="https://render.githubusercontent.com/render/math?math=Q_t"> over time <img src="https://render.githubusercontent.com/render/math?math=t">.</p>

<br>

We expect the distribution of <img src="https://render.githubusercontent.com/render/math?math=Q_t "> to be log-normal as the integral <img src="https://render.githubusercontent.com/render/math?math=\int_0^t R_s "> is normal - as it is a linear combination of normal random variables.

<br>

We can check the distribution of <img src="https://render.githubusercontent.com/render/math?math=Q_t "> for the fixed value of <img src="https://render.githubusercontent.com/render/math?math=t=0 "> by simulating 1000 realisations of <img src="https://render.githubusercontent.com/render/math?math=\log(1_{10}) "> and approximating their empirical distribution using a histogram - shown in Figure 9 below.  From this histogram we can see that the distribution of <img src="https://render.githubusercontent.com/render/math?math=\log(Q_{t}) "> appears to show the characteristic bell curve shape we would expect.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure 10-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 9 - Histogram of empirical distribution of bond price <img src="https://render.githubusercontent.com/render/math?math=Q_t">.</p>

<br>

We can evidence this further by using a Q-Q plot to compare the empirical distribution we have simulated with the normal distribution - shown in Figure 10 below.  From this plot we see clear evidence that the distribution is normal due to the linearity of the points generated.

<br>

<img src="{{< blogdown/postref >}}index_files/figure-html/figure11-1.png" width="75%" style="display: block; margin: auto;" />

<p style="text-align: center;"> 
Figure 10 - Normal Q-Q plot of <img src="https://render.githubusercontent.com/render/math?math=Q_t"> distribution.</p>

<br>

---

## 5. Conclusion


Both models discussed in this project come with several limitations. Firstly, the Black- Scholes model assumes that an option can only be exercised at expiration which limits its use to European options (as US options can be exercised before expiration). The model also makes assumptions that do not tend to hold in real world applications such as that no dividends are paid out during the life of the option, that markets are efficient, that there are no transaction costs in buying the option, that the risk-free rate and volatility of the underlying assets are known and constant and that the returns on the underlying assets are known and constant.\vspace{.4cm}


The main disadvantage of the Vasicek model that has come to light since the global financial crisis is that the model does not allow for interest rates to dip below zero and become negative. This issue has been fixed in several models that have been developed since the Vasicek model such as the exponential Vasicek model and the Cox-Ingersoll-Ross model for estimating interest rate changes and further investigation into these models would be a useful topic of further research.

<br>

---

## References

1. Burgess, N. (2014). An overview of the vasicek short rate model. University of Oxford, Said Business School.

2. F. Black, M. S. (1973). The pricing of options and corporate liabilities. Journal of Political Economy.

3. Hayes, A. (2019). Investopedia - vasicek interest rate model.

4. Hull, J. C. (2003). Options, Futures and Other Derivatives. Number ISBN 0-13-009056-5. Upper Saddle River, NJ: Prentice Hall.

5. Kenton, W. (2020). Investopedia - black scholes model.

6. Seth, S. (2019). Investopedia - circumventing the limitations of black-scholes.

7. Vasicek, O. (1977). An equilibrium characterisation of the term structure. Journal of Fi- nancial Economics, 5:177–188.


