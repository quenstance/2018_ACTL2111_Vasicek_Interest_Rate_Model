# Vasicek Interest Rate Model

## Overview
This project focuses on the implementation and parameter estimation of the **Vasicek Interest Rate Model** using real-world interest rate data from the Australian market. The goal is to estimate the three key model parameters ($\alpha$, $\beta$, and $\sigma_r$) and subsequently use them to calculate and visualize the theoretical **term structure of interest rates** (yield to maturity). For more general details, can refer to the [project web page](https://quenstance.pages.dev/projects/vasicek-interest-rate-model/).


## Methodology
The implementation followed a structured approach:

1.  **Model Discretization:** The continuous Vasicek Stochastic Differential Equation (SDE), $dr=a(b-r)dt+\sigma_{r}dz$, was converted into its discrete-time form: $\Delta r_{t}=r_{t}-r_{t-1}=a(b-r_{t-1})\Delta t+\sigma_{r}\Delta z$. This discrete form implies that the change in the interest rate, $\Delta r$, is normally distributed.

2.  **Maximum Likelihood Estimation (MLE):** The parameters ($a$, $b$, and $\sigma_r$) were estimated by maximizing the Log Likelihood Function.
    * For each observation $\Delta r_t$, the mean ($\mu$) and variance ($\sigma^2$) were defined as $\mu=a(b-r_{t-1})\Delta t$ and $\sigma^{2}=\sigma_{r}^{2}\Delta t$.
    * The probability density function for each observation, $f(\Delta r_t)$, was calculated using the `NORM.DIST` function in Excel.
    * The log likelihood function was then constructed as the summation of the log density functions for all observations, using the Excel function `LN()` to take the natural log of each probability density value. 
    * Optimization was performed in an Excel environment using the Solver tool to find the parameter values that maximize the log likelihood function.

    The log density function for a normally distributed variable $x$ is:
    
$$ \ln f(x) = -\frac{1}{2}\ln(2\pi) - \frac{1}{2}\ln(\sigma^{2}) - \frac{(x-\mu)^{2}}{2\sigma^{2}} $$

3.  **Yield Curve Calculation:** The estimated parameters were used to compute the zero-coupon bond price, $P(t,s)=A(t,s) \times \exp[-B(t,s) \times r(t)]$, using the time $t=0$ interest rate value $r(t)$ (the first entry in the data). The functions $A(t,s)$ and $B(t,s)$ capture the maturity dependency, $s-t$. Finally, the yield to maturity $y(t,s)=-\frac{1}{(s-t)}ln[P(t,s)]$ was calculated and plotted for maturities ranging from 1 to 10 years (in 1-year increments), 20 years, and 30 years.


## Findings
The project successfully estimated the three parameters ($a$, $b$, and $\sigma_r$) using the provided Australian interest rate data and the MLE framework. The estimated parameters were used to generate a suitable table of theoretical yields to maturity. The resulting plot of yield versus maturity period visually demonstrates the term structure of interest rates implied by the Vasicek model, providing a strong understanding of interest rate modeling.

![yield_curve.png](https://github.com/quenstance/2018_ACTL2111_Vasicek_Interest_Rate_Model/blob/829f49d939872d05a39b61ac0d6adb57797850a1/yield_curve.png)

The following parameters were found using the Excel solver:

| Metric | Value |
| :--- | :--- |
| **Maximum Log Likelihood Function Value** | 2156.43849 |
| **Estimated Parameter $\alpha$** | 0.08835 |
| **Estimated Parameter $\beta$** | 0.06716 |
| **Estimated Parameter $\sigma_r$** | 0.00445 |


## How to Use
The data for this project is located in the `S1_2018_ACTL2111_Vasicek_Aus_Data` Excel file within this repository. To replicate the results, you will need to:

1.  **Update your data** in Sheet("Data"), specifically in Columns B and C, to run the calculations.
2.  **Activate the Solver Add-in** in your Excel environment, as it is required to perform the maximum likelihood estimation.


## Limitations
The primary limitation is the inherent assumption of the Vasicek model that the instantaneous variance of the short rate ($\sigma_r^2$) is constant, leading to a normally distributed error term. This constant volatility assumption can be unrealistic, and it theoretically allows the interest rate to become negative, which is generally not observed in practice.

Additionally, the MLE setup simplifies the process by assuming that all observations are independent (which is required for the log likelihood function to be a simple summation), which may not hold perfectly true for highly dependent financial time series data.

---

### Author

**Quenstance Lau**
* **Web Portfolio:** https://quenstance.pages.dev/
* **LinkedIn:** https://www.linkedin.com/in/quenstance/
* **GitHub:** https://github.com/quenstance/