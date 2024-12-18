# Value at Risk (VaR) and Conditional Value at Risk (CVaR) Analysis for Asian Paints

This project explores Value at Risk (VaR) and Conditional Value at Risk (CVaR) for the stock of Asian Paints (ASIANPAINT.NS) using three methodologies: Historical, Parametric, and Monte Carlo Simulation. The updated analysis provides accurate formulas, detailed methodologies, and refined insights.

---

## Table of Contents
- [Value at Risk (VaR) and Conditional Value at Risk (CVaR) Analysis for Asian Paints](#value-at-risk-var-and-conditional-value-at-risk-cvar-analysis-for-asian-paints)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
    - [What is VaR?](#what-is-var)
    - [What is CVaR?](#what-is-cvar)
  - [Key Concepts](#key-concepts)
  - [Methodologies](#methodologies)
    - [Historical Method](#historical-method)
      - [Theory](#theory)
      - [Why Use It?](#why-use-it)
      - [Steps](#steps)
      - [Equations](#equations)
    - [Parametric Method](#parametric-method)
      - [Theory](#theory-1)
      - [Why Use It?](#why-use-it-1)
      - [Steps](#steps-1)
    - [Monte Carlo Simulation Method](#monte-carlo-simulation-method)
      - [Theory](#theory-2)
      - [Why Use It?](#why-use-it-2)
      - [Steps](#steps-2)
  - [Libraries Used](#libraries-used)
  - [Data Description](#data-description)
    - [Stock Selection](#stock-selection)
    - [Data Source](#data-source)
    - [Data Preprocessing](#data-preprocessing)
  - [Implementation Details](#implementation-details)
  - [Visualizations](#visualizations)
    - [1. Distribution of Returns](#1-distribution-of-returns)
    - [2. Monte Carlo Simulation](#2-monte-carlo-simulation)
    - [3. Method Comparison](#3-method-comparison)
  - [Results and Insights](#results-and-insights)
    - [Observations](#observations)
  - [Conclusion](#conclusion)
  - [Future Scope](#future-scope)

---

## Introduction

### What is VaR?
Value at Risk (VaR) quantifies the potential loss in value of a portfolio over a given time period at a specified confidence level. For instance, a 95% 1-day VaR of ₹100,000 means there is a 95% likelihood that losses will not exceed ₹100,000 on any given day.

### What is CVaR?
Conditional Value at Risk (CVaR) measures the average loss given that the loss has exceeded the VaR threshold. It provides insight into the tail risk—losses during extreme market conditions.

---

## Key Concepts

- *Confidence Level*: The probability that losses will not exceed the VaR threshold (e.g., 95%).  
- *Holding Period*: The time frame for risk estimation (e.g., 1 day).  
- *Portfolio Value*: Total investment amount (₹1,000,000 in this analysis).  

---

## Methodologies

### Historical Method

#### Theory  
The Historical Method calculates risk by analyzing historical returns of the stock. Unlike other methods, it does not assume any specific distribution of returns. This approach is practical when historical data is representative of current market conditions.

- *Advantages*:
  - Simple to implement.
  - No assumptions about the distribution of returns.
  - Reflects real market behavior.

- *Limitations*:
  - Results depend on the quality of historical data.
  - Cannot predict unprecedented events or regime changes.

#### Why Use It?  
The Historical Method was chosen as a baseline for comparison because it relies purely on actual past data, avoiding assumptions about return distributions.

#### Steps  
1. Sort historical returns in ascending order.  
2. Identify the quantile at $(1−Confidence Level)$.  
3. Compute CVaR as the average of returns below the VaR threshold.

#### Equations  
- VaR:  
  $$
  \text{VaR}_{\text{Historical}} = \text{Quantile at } (1 - \text{Confidence Level})
  $$
  Where:  
  - $Confidence Level$ : The percentage level at which risk is estimated (e.g., 95%).

- CVaR:  
  $$
  \text{CVaR}{\text{Historical}} = \frac{\sum{r \leq \text{VaR}} r}{N}
  $$
  Where:  
  - $r$: Returns below the VaR threshold.  
  - $N$: Total number of returns below VaR.

---

### Parametric Method

#### Theory  
The Parametric Method, also known as the Variance-Covariance Method, assumes that returns follow a normal distribution. Risk is calculated based on the statistical properties (mean and standard deviation) of returns.

- *Advantages*:
  - Computationally efficient.
  - Closed-form formulas for VaR and CVaR.

- *Limitations*:
  - Assumes normality, which may underestimate extreme risks if returns are skewed or exhibit heavy tails.

#### Why Use It?  
This method is widely used due to its simplicity and computational speed. It provides standardized risk metrics under the assumption of normally distributed returns.

#### Steps  
1. Calculate the mean $\mu$ and standard deviation $\sigma$ of daily returns.  
2. Compute VaR using:
   $$
   \text{VaR}_{\text{Parametric}} = \mu + \sigma \cdot \Phi^{-1}(1 - \text{Confidence Level})
   $$
   Where:  
   - $μ$ : Mean daily return.  
   - $σ$ : Standard deviation of daily returns.  
   - $Φ^-1$: Inverse cumulative distribution function (CDF) of the standard normal distribution.  

3. Compute CVaR using:
   $$
   \text{CVaR}_{\text{Parametric}} = \mu - \frac{\sigma \cdot \phi(\Phi^{-1}(1 - \text{Confidence Level}))}{1 - \text{Confidence Level}}
   $$
   Where:  
   - $ϕ$: Probability density function (PDF) of the standard normal distribution.

---

### Monte Carlo Simulation Method

#### Theory  
Monte Carlo Simulation models risk by generating a large number of potential future returns based on a distribution (typically normal). VaR and CVaR are computed from the simulated data.

- *Advantages*:
  - Flexible and robust for diverse distributions.
  - Captures non-linear risks and extreme scenarios.

- *Limitations*:
  - Computationally intensive.
  - Accuracy depends on input parameters and number of simulations.

#### Why Use It?  
Monte Carlo Simulation is ideal for scenarios where return distributions deviate from normality. It provides a flexible approach to modeling complex risk scenarios.

#### Steps  
1. Simulate \( N \) returns:
   $$
   \text{Simulated Returns} \sim \mathcal{N}(\mu, \sigma^2)
   $$
   Where:  
   - $μ$: Mean daily return.  
   - $σ^2$: Variance of daily returns.

2. Compute VaR as:
   $$
   \text{VaR}_{\text{Monte Carlo}} = \text{Quantile at } (1 - \text{Confidence Level})
   $$

3. Compute CVaR as the average of returns below the VaR threshold:
   $$
   \text{CVaR}{\text{Monte Carlo}} = \frac{\sum{r \leq \text{VaR}} r}{N}
   $$

---

## Libraries Used

- *numpy*: Statistical and mathematical computations.  
- *pandas*: Data manipulation and analysis.  
- *matplotlib* & *seaborn*: Visualization.  
- *scipy.stats*: Probability functions (PDF, CDF).  
- *yfinance*: Fetching historical stock price data.

---

## Data Description

### Stock Selection
Stock: Asian Paints (ASIANPAINT.NS), a key player in the Indian paint industry.

### Data Source
Historical stock prices (January 2020 to December 2024) fetched using yfinance.

### Data Preprocessing
- Calculate daily returns:
  $$
  \text{Daily Returns} = \frac{\text{Adj Close}_t - \text{Adj Close}_{t-1}}{\text{Adj Close}_{t-1}}
  $$
- Remove missing values.

---

## Implementation Details

- Compute *VaR* and *CVaR* using all methods.
- Generate *Monte Carlo Simulations* (10,000 iterations).
- Overlay results on visualizations for clarity.

---

## Visualizations

### 1. Distribution of Returns
- *Histogram* of daily returns with fitted normal distribution.  
- Vertical lines indicating VaR and CVaR thresholds.

### 2. Monte Carlo Simulation
- Simulated return distribution with annotated VaR and CVaR thresholds.

### 3. Method Comparison
- Bar chart comparing VaR and CVaR values across all methodologies.


---

## Results and Insights

| Method       | VaR (₹)  | CVaR (₹)  |
|--------------|----------|-----------|
| Historical   | -24,700   | -39,000    |
| Parametric   | -26,800   | -33,700    |
| Monte Carlo  | -26,900   | -33,400    |

### Observations  
- *Historical Method*: Conservative estimates, dependent on past data.  
- *Parametric Method*: Slightly lower CVaR due to normality assumption.  
- *Monte Carlo Method*: Balances flexibility and risk realism.

---

## Conclusion

This analysis demonstrates how VaR and CVaR help quantify portfolio risks. Each method offers distinct advantages, depending on data characteristics and application needs.

---

## Future Scope

- Use *non-parametric simulations* (e.g., bootstrapping).  
- Incorporate *multi-asset portfolios* and correlation analysis.  
- Extend to *higher confidence levels* and *longer holding periods*.
