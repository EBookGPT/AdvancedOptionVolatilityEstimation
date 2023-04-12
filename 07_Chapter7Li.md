# Chapter 7: Limitations of Black-Scholes Model

## Introduction

Welcome back esteemed readers! In the last chapter, we explored the famous Black-Scholes Model and its various assumptions. In this chapter, we will delve into the limitations of the Black-Scholes Model.

As you may recall, the Black-Scholes Model assumes that stock prices follow a log-normal distribution and that there is no arbitrage opportunity in the market. However, reality is often much messier than models, and the assumptions made by Black-Scholes do not always hold.

To help us delve into this topic, we have a special guest with us today: Myron Scholes. Mr. Scholes is a world-renowned economist and best known for his work on options pricing theory, for which he was awarded the Nobel Memorial Prize in Economic Sciences in 1997.

## The Assumptions Revisited

Before we look at the limitations of Black-Scholes, let's quickly refresh our minds about the assumptions of the model.

Firstly, the model assumes that the stock price follows a log-normal distribution. Secondly, the model assumes that there are no arbitrage opportunities in the market. Finally, the model assumes that the risk-free interest rate is constant and known.

On paper, these assumptions may seem reasonable, but in practice, they are not always accurate. Let's now take a look at some of the limitations of Black-Scholes.

## Limitations of Black-Scholes Model

### 1. Volatility Clustering

In reality, stock price movements tend to exhibit volatility clustering, which means that high volatility periods tend to follow other high volatility periods, and low volatility periods tend to follow other low volatility periods. This phenomenon is not captured by the Black-Scholes Model and can result in inaccurate option prices.

### 2. Skewed Distributions

The Black-Scholes Model assumes that stock prices follow a log-normal distribution, which is symmetric. However, in practice, stock price distributions tend to be skewed. For example, during times of crisis, stock prices may fall sharply, which can lead to a skewed distribution of returns. This can lead to inaccurate option prices.

### 3. Implied Volatility Smile

The implied volatility smile is a phenomenon where implied volatility tends to be higher for options that are further in or out of the money. This is not captured by the Black-Scholes Model, which assumes a constant volatility across all strike prices.

### 4. Transaction Costs and Liquidity

The Black-Scholes Model assumes that there are no transaction costs or liquidity issues in the market. However, in practice, these issues can be significant and can have a material impact on option prices.

## Conclusion

And there we have it! The limitations of the Black-Scholes Model are numerous and complex. While the model remains a popular way to estimate option prices, it is important to keep in mind its limitations and to consider other models that may be more appropriate depending on the circumstances.

We hope you found this chapter informative and would like to thank Myron Scholes for joining us today. In the next chapter, we will explore some advanced option volatility estimation models that address some of the limitations of the Black-Scholes Model. Until then, happy coding!
# Chapter 7: Limitations of Black-Scholes Model

## Epic: The Tale of Icarus and the Black-Scholes Model

In Greek mythology, Icarus was the son of the master craftsman Daedalus. Daedalus fashioned two sets of wings out of wax and feathers for himself and his son so they could escape the clutches of the King of Crete. Before they flew away, Daedalus warned Icarus not to fly too close to the sun, as the wax would melt and the wings would no longer work.

Excited to be flying, Icarus reveled in the freedom of the skies. But as he soared higher and higher, he forgot his father's warning and flew too close to the sun. The wax on his wings melted, and Icarus fell to his death in the sea.

Just like Icarus, the Black-Scholes Model can be susceptible to its own set of limitations. The model assumes a constant risk-free interest rate, symmetric and constant volatility, and no arbitrage opportunities. But in reality, these assumptions may not always hold, leading to inaccurate option prices.

## Special Guest: Myron Scholes

As we saw in the last chapter, Myron Scholes was awarded the Nobel Memorial Prize in Economic Sciences for his work on options pricing theory, which is based on the Black-Scholes Model. But what happens when the Model's assumptions don't hold?

Let's hear what Myron Scholes has to say on this topic.

Myron Scholes: "While the Black-Scholes Model is a popular and widely used model, its assumptions don't always hold in practice. This can lead to inaccurate option prices and negative consequences for investors. It's important to acknowledge and account for these limitations by exploring alternative or advanced models that better capture the complexities of the market."

## Resolution: Overcoming the Limitations

Fortunately, there are a number of advanced option volatility estimation models that can help overcome the limitations of the Black-Scholes Model. These models may use more complex mathematical formulas or take into account additional factors, such as time-varying volatility or skewness in the distribution of stock prices.

For example, the Heston Model is a popular alternative to the Black-Scholes Model that models stock prices and volatility as stochastic processes rather than assuming constant values. Other models, such as the Variance-Gamma Model or the Bates Model, incorporate elements like skew and jump-diffusion to better reflect the behavior of market prices.

By using advanced models that account for the limitations of the Black-Scholes Model, investors can make more informed trading decisions and reduce their risk of loss.

## Conclusion

Just like Icarus flying too close to the sun, the Black-Scholes Model can be susceptible to its own set of limitations. By acknowledging these limitations and exploring alternative or advanced models, we can better capture the complexities of the market and make more informed decisions when trading options.

We hope you enjoyed this chapter and learned something new about the limitations of the Black-Scholes Model. Until next time, happy coding and may your wings stay strong!
# Chapter 7: Limitations of Black-Scholes Model

## Code Explanation: Advanced Option Volatility Estimation

To overcome the limitations of the Black-Scholes Model, advanced option volatility estimation models can be used. These models take into account additional factors, such as time-varying volatility or skewness in the distribution of stock prices.

Below is an example of code used to estimate option prices using the Heston Model, which is a popular alternative to the Black-Scholes Model:

```python
import numpy as np
import scipy.stats as si
import time

class HestonModel:
    def __init__(self, S0, K, T, r, kappa, theta, sigma, rho, v0):
        self.S0 = S0
        self.K = K
        self.T = T
        self.r = r
        self.kappa = kappa
        self.theta = theta
        self.sigma = sigma
        self.rho = rho
        self.v0 = v0

    def generatePaths(self, n, m):
        dt = self.T / m
        sqrdt = np.sqrt(dt)
        r = self.r
        kappa = self.kappa
        theta = self.theta
        sigma = self.sigma
        rho = self.rho
        v0 = self.v0
        S0 = self.S0
        K = self.K

        z1 = np.random.standard_normal((n, m))
        z2 = np.random.standard_normal((n, m))

        v = np.ones((n, m + 1)) * v0
        s = np.ones((n, m + 1)) * S0

        for i in range(1, m + 1):
            v[:, i] = (
                v[:, i - 1]
                + kappa * (theta - np.maximum(v[:, i - 1], 0)) * dt
                + sigma * np.sqrt(np.maximum(v[:, i - 1], 0)) * sqrdt * z1[:, i - 1]
            )

            s[:, i] = (
                s[:, i - 1]
                * np.exp((r - np.maximum(v[:, i - 1], 0) / 2) * dt)
                * np.exp(np.sqrt(np.maximum(v[:, i - 1], 0)) * rho * sqrdt * z1[:, i - 1])
            )

        self.paths = s
        self.volatility = np.sqrt(v)

    def calcOptionPrice(self):
        S = self.paths
        T = self.T
        K = self.K
        r = self.r
        v = self.volatility

        # Terminal price distribution
        log_S_T = np.log(S[:, -1])
        m = len(log_S_T)
        mu_T = log_S_T + (r - np.maximum(v[:, -1] ** 2, 0) / 2) * T
        sigma_T = v[:, -1] * np.sqrt(T)

        # Calculate option price using Monte Carlo simulation
        option_prices = np.exp(-r * T) * (
            np.maximum(S[:, -1] - K, 0)
        )  # for the case where T = 0
        for i in range(m):
            d1 = (np.log(S[:, 0] / K) + (r + v[:, i] ** 2 / 2) * T) / (
                v[:, i] * np.sqrt(T)
            )
            d2 = d1 - v[:, i] * np.sqrt(T)

            ND1 = si.norm.cdf(d1)
            ND2 = si.norm.cdf(d2)

            option_price_i = S[:, 0] * ND1 - np.exp(-r * T) * K * ND2
            option_prices = np.maximum(option_prices, option_price_i)

        self.option_prices = option_prices.mean()
```

In this code, we define the Heston Model with parameters such as stock price, strike price, time to maturity, risk-free interest rate, and various volatility parameters. We then generate simulated paths using the model and compute the corresponding option prices using Monte Carlo simulation.

By using advanced models such as the Heston Model, we can better capture the complexities of the market, including volatility clustering and skewed distributions, and improve our ability to estimate option prices.