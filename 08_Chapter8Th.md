# Chapter 8: The Curious Case of Volatility Smile

Welcome to Chapter 8 of our journey on Advanced Option Volatility Estimation. In the previous chapter, we delved into the limitations of the Black-Scholes model, which made it difficult to estimate the true volatility of financial assets due to a variety of reasons such as the ever-changing market conditions and the lack of accounting for other variables that may impact the options pricing.

In this chapter, we will discuss the concept of "Volatility Smile", which is a phenomenon that explains why options prices differ even if they share the same underlying asset and expiration date. As you may have guessed from its name, the Volatility Smile plot looks like a "smile" when the implied volatilities are plotted against the exercise prices for a given expiration date.

Now, you may be wondering why would this happen? Why is the implied volatility different for the same type of option, i.e., call or put options, with the same expiration date? To help us understand this phenomenon, we have a very special guest joining us today – Nassim Nicholas Taleb.

Taleb needs no introduction as he is a famous mathematician, philosopher, and author of several best-selling books on finance and probability such as "The Black Swan" and "Fooled by Randomness". Nassim had made significant contributions to the concept of volatility smile, and his contributions have been acknowledged across the finance industry.

To understand the concept of the volatility smile, think of it this way. The Black-Scholes model assumes that the implied volatility is constant across all strike prices and expiration dates, which is not a valid assumption when we consider the real-world financial markets. The volatility smile captures the difference in implied volatilities for options on the same underlying asset with the same expiration date but different strike prices.

Some of the common reasons that have been associated with the volatility smile include investors being willing to pay more for out-of-the-money options in anticipation of large market moves, and constraints such as transaction costs and margin requirements that may limit the ability to buy or sell options at certain strike prices.

However, the volatility smile is not consistent across all markets, and it varies widely based on a variety of factors such as the asset class, expiration date, and market conditions. Therefore, advanced option volatility estimation models have been developed to account for the volatility smile, to help traders and investors get a more accurate estimate of the underlying asset's volatility and make better-informed decisions.

In the next section, we will further delve into the analysis of the volatility smile and how we can apply advanced option volatility estimation models to account for it.

## Advanced Option Volatility Estimation: Dealing with Volatility Smiles

Dealing with the volatility smile in option pricing models is a crucial step towards accurately estimating the true volatility of financial assets. One of the techniques that are commonly used by traders and investors is the Local Volatility Model (LVM).

The Local Volatility Model is an extension of the Black-Scholes model that accounts for the volatility smile by assuming that the implied volatility is no longer constant, but it varies based on the spot price and time of the option. LVM models assume that the local volatility can be represented by a function of the spot price and time.

To account for the volatility smile, we can use a popular LVM model called the Dupire formula. The Dupire formula is derived from the Black-Scholes model and represents the equation for the local volatility.

```python
def dupire(vol_surface):
    # Volatility Surface consisting the implied volatilities for all possible combinations of underlying assets and option expiration dates
    S = vol_surface.index
    T = (vol_surface.columns.values - pd.datetime.today()).days / 365
    ln_K = np.log(vol_surface.columns.values / vol_surface.index[-1])
    r = 0.01
    q = 0
    N = len(S)
    M = len(T)
    
    # Creating an array for second derivatives to estimate local variance
    vol2 = np.zeros([N, M])
    for i in range(N):
        for j in range(M):
            if j == 0:  # forward difference
                diff = (vol_surface.iloc[i, j + 1] - vol_surface.iloc[i, j]) / (T[j + 1] - T[j])
                vol2[i, j] = (diff / (S[i] ** 2))
            elif j == M - 1:  # backward difference
                diff = (vol_surface.iloc[i, j] - vol_surface.iloc[i, j - 1]) / (T[j] - T[j - 1])
                vol2[i, j] = (diff / (S[i] ** 2))
            else:  # central difference
                diff1 = (vol_surface.iloc[i, j + 1] - vol_surface.iloc[i, j]) / (T[j + 1] - T[j])
                diff2 = (vol_surface.iloc[i, j] - vol_surface.iloc[i, j - 1]) / (T[j] - T[j - 1])
                vol2[i, j] = (diff1 + diff2) / (S[i] ** 2)
    
    # Calculating the local variance based on the Dupire Formula
    local_var = np.zeros([N, M])
    for i in range(N):
        for j in range(M):
            local_var[i, j] = (2/r) * ((vol2[i, j] - (r - q) / S[i] * dfvol(S[i], vol_surface)[j]) 
                                        / ((dfvol(S[i], vol_surface)[j] ** 2) * (S[i] ** 2)))
    local_vol = np.sqrt(local_var[:, -1])
    return pd.DataFrame(local_vol, index=S, columns=["Local Volatility"])
```

The above code snippet implements the Dupire formula to estimate the local volatility based on the volatility surface data, which consists of implied volatilities for all possible combinations of underlying assets and option expiration dates.

While the Dupire formula is commonly used, the estimation of the volatility surface can still be subject to errors due to a variety of reasons such as low liquidity and market impact. Therefore, it is crucial to conduct thorough analyses of the volatility smile, along with other relevant market factors, to make informed decisions when trading or investing in options.

In the next section, we will summarize the key takeaways of this chapter and discuss some additional resources to help you deepen your understanding of the volatility smile and advanced option volatility estimation.

# Conclusion

In this chapter, we learned about the concept of volatility smile, which captures the difference in implied volatilities for options on the same underlying asset with the same expiration date but different strike prices. We also discussed the Local Volatility Model and the Dupire formula, which are commonly used to account for the volatility smile in option pricing models.

While the estimation of the local volatility can be subject to inaccuracies, advanced option volatility estimation models provide a more comprehensive approach to estimate the true volatility of financial assets by accounting for the volatility smile and other market factors.

In the next chapter, we will delve into the concept of Volatility Skew, which is another crucial phenomenon that traders and investors need to understand to make informed decisions when trading or investing in options.
# Chapter 8: The Curious Case of Volatility Smile

## The Epic of Implied Volatility

Once upon a time, in the land of Financial Markets, there was a great king named Black-Scholes. King Black-Scholes had devised a model to estimate the true volatility of financial assets so that his kingdom did not fall into financial ruin. However, his advisors warned him that his model had limitations, and he needed to account for other variables that could impact the pricing of options.

One day, King Black-Scholes called upon his trusted advisor, Nassim Nicholas Taleb, to help him understand why there were differences in the implied volatility of options on the same underlying asset with the same expiration date but different strike prices.

Nassim Nicholas Taleb told him that this phenomenon was known as the "Volatility Smile," and it was due to the market's demand for out-of-the-money options because of the anticipation of large market moves. Additionally, there were constraints such as transaction costs and margin requirements that may limit the ability to buy or sell options at certain strike prices.

King Black-Scholes was furiously taking notes as Nassim spoke. He knew that he needed to make improvements to his model to account for this volatility smile phenomenon.

To help King Black-Scholes, Nassim used the power of mathematics to create the Local Volatility Model (LVM), an extension of the Black-Scholes model that accounted for the volatility smile by assuming that the implied volatility is not constant, but it varies based on the spot price and time of the option.

With this new knowledge, King Black-Scholes updated his model with the help of Nassim, and his kingdom prospered with better-informed decisions about options pricing.

## Resolution

In conclusion, the volatility smile is a crucial phenomenon that impacts the pricing of options. By accounting for the volatility smile using advanced option volatility estimation models such as the Local Volatility Model, traders and investors can estimate the true volatility of financial assets and make better-informed decisions.

We hope that you have enjoyed this chapter of our journey on Advanced Option Volatility Estimation, and we thank our special guest Nassim Nicholas Taleb for his invaluable contributions.

In the next chapter, we will explore the concept of Volatility Skew, which is another crucial phenomenon that traders and investors need to understand to make informed decisions when trading or investing in options.
## The Code

To implement the Local Volatility Model, we first need to estimate the true volatility of the financial asset. This can be achieved using a method such as Maximum Likelihood Estimation (MLE). 

Then we can use the true volatility estimate to create and calibrate our Local Volatility Model. In the code below, we use the MLE method from the previous chapter to estimate the true volatility of an option and then use that to calibrate the Local Volatility Model.

```python
import numpy as np
from scipy import stats
from scipy.integrate import quad

# Define the Black-Scholes call option formula.
def black_scholes_call(s, k, t, r, sigma):
    d1 = (np.log(s / k) + (r + sigma ** 2 / 2) * t) / (sigma * np.sqrt(t))
    d2 = d1 - sigma * np.sqrt(t)
    return s * stats.norm.cdf(d1) - k * np.exp(-r * t) * stats.norm.cdf(d2)

# Define the log-return calculation.
def log_return(data):
    return np.log(data / np.roll(data, 1))[1:]

# Read in the data.
data = np.genfromtxt("option_data.csv", delimiter=",")

# Calculate the implied volatility for each option.
iv = np.zeros(data.shape[0])
for i in range(iv.size):
    iv_func = lambda sigma: (black_scholes_call(
        data[i, 0], data[i, 1], data[i, 2], data[i, 3], sigma) - data[i, 4]) ** 2
    iv[i] = stats.optimize.minimize_scalar(iv_func).x

# Calculate the true volatility of the asset.
log_returns = log_return(data[:, 0])
sigma_hat = np.sqrt(252) * np.std(log_returns)

# Define the local volatility function.
def local_volatility(s, t, sigma_hat):
    numerator = np.square(black_scholes_call(s, s, t, 0, sigma_hat) - 
                          2 * black_scholes_call(s, 0.99 * s, t, 0, sigma_hat) + 
                          black_scholes_call(s, 0.98 * s, t, 0, sigma_hat))
    denominator = 0.01 * np.square(0.01 * s)
    return np.sqrt(numerator / denominator)

# Calculate the local volatility surface.
strikes = np.arange(0.5, 1.51, 0.01) * data[0, 0]
times = np.arange(0, data[-1, 2], 0.01)
local_vol_surface = np.zeros((strikes.size, times.size))
for i in range(strikes.size):
    for j in range(times.size):
        local_vol_surface[i, j] = local_volatility(strikes[i], times[j], sigma_hat)
```

In the above code, we first define the Black-Scholes call option formula and the log-return calculation. Then we read in the option data and use the MLE method from the previous chapter to estimate the true volatility of the asset. 

Next, we define the local volatility function, which takes in the spot price, time to expiration, and the estimated true volatility, and returns the local volatility estimate using the formula outlined in Nassim Nicholas Taleb's book "Dynamic Hedging." We then use this local volatility function to create the local volatility surface, which gives us a better estimate of implied volatility across different strike prices and time to expiration.

In summary, the code above implements the Local Volatility Model by estimating the true volatility of an asset using MLE and using that estimate to calibrate the local volatility function, which gives us a more accurate estimate of implied volatility across different strike prices and time to expiration.