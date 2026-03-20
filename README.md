# Parametric Rainfall Insurance — Distribution Modelling and Simulation Framework

## 1. Problem Motivation

From my professional experience, I got an opportunity to look at certain parametric insurance products and how they differ from traditional insurance products. I got an opportunity to look at the monitoring of these portfolios, but did not get a chance to price them. Parametric insurance products are increasingly used to provide faster payouts and claim settlements based on observable triggers such as rainfall levels. 

To further research more on how these products are priced, I used a parametric rainfall product and tried simualting insurance losses over a period of one year using time series and stochastic models. However, unlike traditional insurance, payouts depend on an index rather than actual losses, making the modelling of the underlying variable critical.

Rainfall behaviour is extremely uncertain and often exhibits variability that is not easy to capture using deterministic assumptions. This creates challenges in designing triggers and understanding the distribution of potential payouts. There is basis risk in these parametric insurance products, and it is difficult to model them. 

This project explores how rainfall distributions and stochastic simulation can be used to model losses in a parametric insurance framework.

---

## 2. Objective

To model rainfall amounts using statistical distributions like Tweedie & Gamma and simulate rainfall paths in order to estimate the distribution of insurance payouts under a parametric rainfall policy.

---

## 3. Data & Inputs

- Historical daily rainfall data (Miami, FL) for the past 30 years from 1995 to 2024. 

---

## 4. Modelling Approach

### Rainfall Distribution Modelling
- 30 years of daily rainfall data analysed to fit Tweedie and Gamma distributions
- Compare the fit of Gamma and Tweedie to select the distribution
- Distribution selection based on the ability to capture observed variability and skewness
- Calibration of parameters for the Tweedie distribution
- Bootstrapped K-S test for evaluating the fit of the Tweedie distribution  

---

### Stochastic Rainfall Simulation
- AR(1) Time Series model used to simulate the parameter $\mu_{t}$ of the Tweedie distribution for each day for the next one year
- Using the simulated $\mu_{t}$ path to generate a random value from the Tweedie distribution with parameter $\mu_{t}$, $p$, and $\phi$. Parameters $p$ and $\phi$ are kept fixed across all days and paths and calibrated in the first notebook
- Multiple simulation paths generated to capture variability in rainfall outcomes  

---

### Insurance Loss Modelling
- Parametric insurance structure is defined using thresholds from observed rainfall data
- Payouts triggered when rainfall exceeds a predefined threshold
- Losses calculated across simulated rainfall paths  

---

## 5. Key Insights

- The choice of probability distribution plays an important role in capturing tail behaviour, along with calibration of the distribution
- The fitted Tweedie distribution has heavy tails, which overestimate rainfall on a given day - a better approach would be to model the occurrence of rainfall and rainfall intensity using separate distributions
- Rainfall variability leads to a wide distribution of possible payouts, even under similar average conditions  
- Small changes in trigger thresholds and exit points can significantly impact expected losses on the insurance policy  

---

## 6. Limitations

- Model assumes independence in rainfall across periods  
- The model does not explicitly capture extreme weather dynamics or climate trends  
- Basis risk (difference between actual loss and index payout) is not explicitly analysed  

---

## 8. Repository Structure

- `01_RainfallDistribution` — Distribution fitting and exploratory analysis  
- `02_StochasticRainPath` — Simulation of rainfall paths  
- `03_InsuranceLossesSimulation` — Parametric insurance payout modelling  

---

## 9. Takeaway

This project emphasizes how crucial distributional assumptions and stochastic simulation are when modeling parametric insurance products, especially when it comes to capturing tail risk and uncertainty in climate driven occurrences. The project could be extended to model a wide range of climate risks.

---

## Libraries

`numpy` `pandas` `matplotlib` `statsmodels` `scipy` `tweedie` 

## How to Run

Run the notebooks in order: `01` → `02` → `03`. Notebook 03 imports functions from notebooks 01 and 02 `import_ipynb`.

