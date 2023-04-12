# Chapter 9: Volatility Skew

In the previous chapter, we explored the concept of the volatility smile, which indicates that the implied volatility of options varies depending on the strike price. Another important concept in option pricing is the volatility skew. Volatility skew, like the volatility smile, reveals a pattern in the implied volatility, but this time the pattern occurs with respect to the expiration date of the option contract.

The volatility skew is the difference in implied volatility between out-of-the-money (OTM), at-the-money (ATM), and in-the-money (ITM) options. In general, the volatility skew shows that options with a higher implied volatility tend to have a higher strike price. However, this is not always the case, and the volatility skew can differ depending on the underlying asset and the time to expiration.

Volatility skew is an essential concept in options pricing because it can affect the profitability of an option trading strategy. Therefore, taking the skew into account when pricing options can significantly improve the accuracy of the pricing model.

In this chapter, we will dive deeper into the topic of volatility skew and explain how to estimate it using Advanced Option Volatility Estimation code. We will analyze how volatility skew can help traders make better trading decisions and discuss strategies to mitigate the risks associated with volatility skew.

Let us embark on this journey of unraveling the mysteries of the volatility skew and discover how it can benefit every options trader.
# Chapter 9: Volatility Skew

## The Mystery of the Unexpected Price Movements

Sherlock Holmes had just returned to his office when Dr. John Watson showed him a newspaper article talking about a sudden and unexpected jump in the price of a stock that had been trading sideways for weeks. Watson asked Holmes what could have caused such a sharp rise in such a short time.

Holmes looked at the article and scratched his chin. He then asked Watson if he knew that the stock had a highly skewed volatility profile. Watson was confused and asked Holmes to explain.

Holmes then explained that a highly skewed profile meant that the volatility of the stock for options with different strike prices or expirations could vary significantly. Holmes suspected that the sudden price jump was due to someone buying options that were highly affected by the volatility skew.

Holmes and Watson decided to investigate the trade record of the stock options, and after some analysis, they found unusual activity in the options market. There were many buyers buying a large number of OTM options with a strike price well above the current market price.

After some intense investigation, Holmes and Watson found that a large financial institution was behind the purchases. They had observed a sharp increase in demand for the product and decided to take advantage by strategically buying the OTM options.

The financial institution had taken the volatility skew into account and made their profits by exploiting the very asset's volatility profile that others had failed to exploit for weeks.

## The Resolution

Holmes had successfully solved the mystery, but he had also realized how important it was to be aware of the volatility skew when trading options. It is essential to understand that using the option pricing models that don't account for volatility skew can lead to mispricings and potential losses.

It is vital to note that there are various methods of estimating volatility skew, such as the technique described in the [“Pricing Options with Skewness and Kurtosis”](https://www.jstor.org/stable/2646678?seq=1) by N. Bakshi et al. Holmes and Watson had managed to use the techniques to uncover the mystery behind the sudden price move.

Therefore, traders must learn about how to obtain the values of volatility skew accurately for the best option trading strategies to minimize risks and maximize profits. The lesson is to be alert when market prices move unexpectedly, and keep the concept of volatility skew in mind as it can provide a clue to the underlying reason for the price move.
# Chapter 9: Volatility Skew

## Code to Resolve the Sherlock Holmes Mystery

To estimate the volatility skew, we can use the Black-Scholes-Merton (BSM) option pricing model. BSM is a commonly used model to price options, and it assumes that the log returns of the underlying asset follow a normal distribution. We can estimate the implied volatility of options by solving the BSM model for the volatility of the underlying asset that matches the current market prices of the options.

Here is the code that Holmes and Watson could have used to estimate the volatility skew:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import norm
from scipy.optimize import minimize

class BSModel:
    def __init__(self, spot, rate, div_yield, option_type):
        self.spot = spot
        self.rate = rate
        self.div_yield = div_yield
        self.option_type = option_type

    def d1(self, strike, t, vol):
        return (np.log(self.spot / strike) + (self.rate - self.div_yield + 0.5 * vol ** 2) * t) / (vol * np.sqrt(t))

    def d2(self, strike, t, vol):
        return self.d1(strike, t, vol) - vol * np.sqrt(t)

    def price(self, strike, t, vol, *args):
        if self.option_type == "call":
            return self.spot * np.exp(-self.div_yield * t) * norm.cdf(self.d1(strike, t, vol)) \
                   - np.exp(-self.rate * t) * strike * norm.cdf(self.d2(strike, t, vol))
        elif self.option_type == "put":
            return np.exp(-self.rate * t) * strike * norm.cdf(-self.d2(strike, t, vol)) \
                   - self.spot * np.exp(-self.div_yield * t) * norm.cdf(-self.d1(strike, t, vol))


def IV_calc(option_type, spot, strike, t, rate, div_yield, premium):
    def target_func(vol, premium, option_type, spot, strike, t, rate, div_yield):
        opt = BSModel(spot, rate, div_yield, option_type)
        return opt.price(strike, t, vol) - premium

    res = minimize(target_func, 0.25, method="SLSQP", args=(premium, option_type, spot, strike, t, rate, div_yield))
    return res.x[0]


def skew(option_type, spot, t, rate, div_yield, df):
    delta = 0.1
    strikes = np.arange(delta * spot, 2 * spot, delta * spot)
    implied_vols = np.array(
        [IV_calc(option_type, spot, x, t, rate, div_yield, df[(df.index == t) & (df["Strike"] == x)]["midpoint"].values[0])
         for x in strikes])
    moneyness = np.log(strikes / spot)
    coeffs = np.polyfit(moneyness, implied_vols, 3)
    return coeffs[2]

```

This code snippet uses the Black-Scholes-Merton option pricing model to estimate the implied volatility of options according to their premiums. 
The `IV_calc` function is used to calculate the implied volatility of each option, given its given market price.
The `skew` function then finds the coefficients of the quadratic fit through the implied volatility for options based on their moneyness, which is the logarithm of the ratio of the spot price to the strike price. 

Holmes and Watson could have used this function to calculate the implied volatility of options for different moneyness and expirations and found that the implied volatility for OTM options with higher strikes was increasing. They could have then used this information to confirm the presence of the volatility skew and to solve the mystery of the price jump.

By using this estimated volatility skew, traders can better manage the risk involved in trading options and develop more robust trading strategies. More advanced techniques for estimating volatility skew can be found in a study called ["Option pricing with skewness and kurtosis"](https://www.jstor.org/stable/2646678?seq=1) by N. Bakshi et al.