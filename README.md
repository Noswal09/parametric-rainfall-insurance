# Parametric Rainfall Insurance — Stochastic Loss Simulation for Weather-Linked Products 

## 1. Problem Motivation

Parametric Insurance is a non-traditional insurance product that pays out an agreed sum based on the magnitude of a specific, pre-defined event (like earthquake or excessive rainfall), rather than on actual assessed incurred losses. 

From my professional experience, I got an opportunity to look at certain parametric insurance products and how they differ from traditional insurance products. I observed several challenges when modelling these products. Parametric insurance products are increasingly used to provide faster payouts and claim settlements based on observable triggers such as rainfall levels. Unlike traditional insurance, payouts depend on an index rather than actual losses, making the modelling of the underlying variable critical. 

Rainfall-based parametric products present challenges because the daily rainfall exhibits several non-trivial features like:
- zero inflation (most days have 0 precipitation)
- temporal dependence (wet and dry spells)
- heavy-tailed distributions (to model extreme rainfall events)
- seasonal variation in mean levels

Standard deterministic or single-distribution approaches are insufficient to capture these characteristics simultaneously. This creates model risk in estimating payout distributions, particularly in the tails where insurance losses are concentrated.

Motivated by this, the project evaluates alternative stochastic frameworks for modelling rainfall and examines their implications for parametric insurance pricing.

---

## 2. Objective

To assess how different stochastic representations of the rainfall process impact the distribution of parametric insurance losses by comparing two approaches:
- **Approach A**: A Tweedie distribution fitted to all rainfall data, including dry days, with the mean parameter varying daily through an AR(1) driven process capturing temporal dependence in rainfall amounts
- **Approach B**: Decomposes rainfall into occurrence and intensity. A Markov Chain model determines whether it rains on a given day, capturing wet day persistence, and a Gamma distribution models rainfall intensity on wet days with a time-varying scale parameter driven by the same AR(1) mean paths as Approach A.

---

## 3. Data & Inputs

- Historical daily rainfall data (Miami, FL) for the past 22 years from 2003 to 2024. 

---

## 4. Modelling Approach

### Rainfall Distribution Modelling

- Fitting the necessary distributions required in both approaches
- **Approach A**: Tweedie Distribution fitted to all rainfall data:
    - 22 years of daily rainfall data analysed to fit Tweedie and Gamma distributions
    - Compare the fit of Gamma and Tweedie to select the distribution
    - Distribution selection based on the ability to capture observed variability and skewness
    - Calibration of parameters for the Tweedie distribution
    - Bootstrapped K-S test for evaluating the fit of the Tweedie distribution  
- **Approach B**: Occurrence/Severity Framework:
    - Markov Chain Transition Matrix fit on historical rainfall data
    - Gamma distribution fit on non-zero rainfall amounts
    - Fit of the Gamma distribution assessed using QQ-plot  

---

### Stochastic Rainfall Simulation
- Simulating paths for the mean of the distribution of daily rainfall using the stochastic process: 

<div align="center">

$\mu_{t} = \bar{\mu_{t}} + X_{t}$. where, $\bar{\mu_{t}}$ is the observed average daily rainfall across 22 years data

</div>

- Fitting a AR(1) time series to $X_{t}$ and then simulating paths for $\mu_{t}$
-  Using these $\mu_{t}$ paths, we simulate rainfall on a given day using two approaches:
    - **Approach A**: Simulate daily rainfall from a Tweedie distribution using time varying $\mu_{t}$ while keeping the power parameter and $\phi$ fixed
    - **Approach B**: Uses a Markov Chain to decide whether it rains on a given day and then draws rainfall intensity from a Gamma distribution with a time-varying scale parameter calibrated to the conditional mean on wet days.

---

### Insurance Loss Modelling
- Parametric insurance structure is defined using thresholds from observed rainfall data
- Payouts triggered when rainfall exceeds a predefined threshold
- Losses calculated across simulated rainfall paths for both approaches
- Sensitivity to triggers is tested

---

## 5. Key Insights

- Key results from the simulations are summarised below:

| | Tweedie | Occurrence/Gamma |
|---|---|---|
| Expected Annual Loss | $23,499 | $25,079 |
| Model Difference | 6.7% | |
| Trigger Exceedance | 33.9% | 35.7% |

- The close agreement in expected losses masks important structural differences between the two approaches
- The Tweedie approach:
    - Provides a better overall fit to rainfall distribution
    - Captures aggregate behaviour more effectively
    - Slightly overestimates the mean rainfall
- The Occurrence/Gamma approach:
    - Tracks conditional mean behaviour more accurately
    - Underestimates extreme rainfall events due to fixed shape parameter assumptions
- From an insurance perspective:
    - Similar expected losses may still lead to different tail risk profiles
    - Underestimation of extremes can result in systematic underpricing for high-trigger contracts
- Small changes in trigger thresholds materially impact expected losses, emphasising the importance of contract design alongside model choice

---

## 6. Limitations

- Monthly alpha estimation showed the shape parameter is stable across seasons 
- AR(1) assumes Gaussian innovations, which may underestimate extreme rainfall deviations
- Basis risk (difference between actual loss and index payout) is not explicitly modelled  
- Analysis uses a single weather station and does not capture spatial rainfall dependence across the region

---

## 8. Repository Structure

- `01_RainfallDistribution` — Distribution fitting and exploratory analysis  
- `02_StochasticRainPath` — Simulation of rainfall paths  
- `03_InsuranceLossesSimulation` — Parametric insurance payout modelling  

---

## 9. Takeaway

This project demonstrates that in parametric insurance, modelling the underlying stochastic process is central to pricing accuracy. Even when different models produce similar expected losses, their assumptions can lead to materially different representations of risk, particularly in the tails. The project could be extended to model a wide range of climate risks.

---

## Libraries

`numpy` `pandas` `matplotlib` `statsmodels` `scipy` `tweedie` 

## How to Run

Run the notebooks in order: `01` → `02` → `03`. Notebook 03 imports functions from notebooks 01 and 02 `import_ipynb`.

