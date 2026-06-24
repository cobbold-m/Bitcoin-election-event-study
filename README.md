# Bitcoin-election-event-study
## Did the 2024 US Presidential Election Generate Abnormal On-Chain Bitcoin Activity?

An event-study analysis testing whether the 2024 US presidential election moved Bitcoin's network economics, or only its price.

---
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat&logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat&logo=matplotlib&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=flat&logo=googlecolab&logoColor=white)

## Overview

This project tests whether the 2024 US presidential election (5 November 2024) produced abnormal on-chain Bitcoin activity, or only a speculative price response. Trump campaigned on a strategic Bitcoin reserve and lighter regulatory oversight (Amsden, 2024), and the analysis asks whether that political signal moved the network economics or just the price. Using an event-study framework across four daily metrics, transaction count, total fees, miner revenue, and BTC price, it separates a price effect from a genuine demand-side effect. This project was completed as part of the [ICM318](https://www.reading.ac.uk/modules/documents?acyear=2025%252f6&modcode=ICM318&schoolcode=HBS%20ICM%20W) module at Henley Business School.

---

## Data

Daily aggregated on-chain metrics are drawn from `BlockData_2020_2025.csv`, covering transaction count, total fees (USD), miner revenue (USD), and BTC price (USD). Miner revenue is decomposed into block subsidy and fee revenue to isolate the demand component from the price-mechanical component.

The 30-day pre-election window (6 Oct to 4 Nov 2024) is compared against the 30-day post-election window (6 Nov to 5 Dec 2024), benchmarked against the same calendar window in 2023. The 2023 baseline is not structurally neutral, Bitcoin traded between $27,000 and $42,000, well below the 2024 pre-election mean of $66,662, but the SAC approach mitigates this by normalising against control-period standard deviation rather than level.

---

## Methodology

An event-study framework (Fama et al., 1969; MacKinlay, 1997) is applied to each metric. The headline statistic is the Standardised Abnormal Change (SAC), which normalises the pre-to-post change against the control-period standard deviation, partially controlling for the structural gap between the 2023 and 2024 price regimes.

$$SAC_m = \frac{\bar{Y}_{m}^{\,post} - \bar{Y}_{m}^{\,pre}}{\sigma_{m}^{\,control}}$$

Significance is assessed with a Welch t-test and a Mann-Whitney U test. Persistence is tracked through cumulative abnormal returns at days 1-7, 1-14, and 1-30, and price-metric relationships are checked with Pearson correlations.

$$CAR_m(1,h) = \sum_{t=1}^{h} AR_{m,t}$$

<div align="center">

| Term | Description |
|------|-------------|
| $\bar{Y}_{m}^{\,pre}$ | Pre-election mean of metric m (6 Oct to 4 Nov 2024) |
| $\bar{Y}_{m}^{\,post}$ | Post-election mean of metric m (6 Nov to 5 Dec 2024) |
| $\sigma_{m}^{\,control}$ | Standard deviation of metric m over the 2023 control window |
| $SAC_m$ | Standardised Abnormal Change for metric m |
| $AR_{m,t}$ | Abnormal change of metric m on day t relative to control |
| $CAR_m(1,h)$ | Cumulative abnormal change over the first h days |

</div>
<p align="center"><em>Table 1: Variable Definitions</em></p>

---

## Empirical Results

The headline results separate a persistent price response from the absence of any genuine demand-side abnormality. Price and miner revenue are significant at the 1% level, but fees, the cleaner proxy for on-chain demand, are statistically indistinguishable from zero.

<div align="center">

| Metric | SAC | t-stat | p (t) | p (MWU) | Sig. at 5% |
|--------|-----|--------|-------|---------|------------|
| Transaction count | −0.767 | 4.228 | 0.0001 | 0.0001 | Yes |
| Total fees (USD) | −0.029 | 0.361 | 0.7201 | 0.4553 | No |
| Miner revenue (USD) | 1.457 | −8.183 | 0.000 | 0.000 | Yes |
| BTC price (USD) | 5.878 | −15.998 | 0.000 | 0.000 | Yes |

</div>
<p align="center"><em>Table 2: Standardised Abnormal Change and Significance Tests, 30-day Pre vs Post 2024 US Presidential Election</em></p>

---

### Bitcoin Price

The post-election mean of $91,017 represents a 36.5% increase from $66,662, with an SAC of 5.88, roughly 6σ above control-period volatility. Both the Welch t-test and the Mann-Whitney U test reject the null at the 1% level, consistent with Tan and Wong (2025), who document persistent price effects following the 2024 result. Price never reverted to its pre-election range across the full 30-day window, confirming full persistence.

<p align="center">
  <img src="images/fig1_price.png" alt="Figure 1: BTC Price Series, Pre vs Post Election with 2023 Control Overlay" width="600"/>
</p>
<p align="center"><em>Figure 1: BTC Price Series, Pre vs Post Election with 2023 Control Overlay</em></p>

---

### Miner Revenue

Miner revenue rose 29.2% post-election (SAC = 1.46), but this reflects price appreciation rather than network demand. The daily block subsidy accounted for $40.96m of the $42.79m post-election total, leaving a fee share of just 3.56%. Easley, O'Hara and Basu (2019) establish fee income as the more dependable proxy for on-chain demand pressure, and by that measure no demand-side response occurred. The Pearson correlation between price and miner revenue strengthened to r = 0.73 post-election, confirming the relationship is price-mechanical.

<div align="center">

| Metric | Value |
|--------|-------|
| Mean daily subsidy (USD) | 40,957,510.91 |
| Mean daily fee revenue (USD) | 1,830,212.00 |
| Mean total miner revenue (USD) | 42,787,722.91 |
| Fee share of revenue (%) | 3.56 |
| Pre-election mean fees (USD) | 1,554,120.95 |
| Post-election mean fees (USD) | 1,467,112.20 |
| Pre to post change in fees (%) | −5.60 |

</div>
<p align="center"><em>Table 3: Miner Revenue Decomposition, Post-Election Window</em></p>

<p align="center">
  <img src="images/fig2_miner_revenue.png" alt="Figure 2: Miner Revenue Decomposed into Subsidy and Fees" width="600"/>
</p>
<p align="center"><em>Figure 2: Miner Revenue Decomposed into Subsidy and Fees</em></p>

---

### Transaction Activity

Transaction count fell 16.8% post-election (SAC = −0.77), diverging sharply from price. The 7-day rolling mean reverted to the pre-election band by day 13, suggesting a short-lived disruption. The Pearson correlation between price and transaction count is effectively zero post-election (r = −0.08, p = 0.68), confirming a decoupling between price and network utilisation (Aalborg et al., 2023). Cahill et al. (2024) find that ETF flows and derivatives trading can drive price without generating proportional on-chain activity, which is precisely the pattern observed here.

<p align="center">
  <img src="images/fig3_transaction_count.png" alt="Figure 3: Transaction Count with 7-Day Rolling Mean" width="600"/>
</p>
<p align="center"><em>Figure 3: Transaction Count with 7-Day Rolling Mean</em></p>

---

### Transaction Fees

Total fees fell 5.6% post-election (SAC = −0.03), statistically indistinguishable from zero. Since fees reflect users bidding competitively for block space, their absence confirms that the post-election capital inflows did not reach the blockchain. The largest fee spikes in the surrounding period coincided with the April 2024 halving and the January 2024 spot ETF launch, not the election.

<p align="center">
  <img src="images/fig4_fees.png" alt="Figure 4: Total Transaction Fees with Halving and ETF Launch Markers" width="600"/>
</p>
<p align="center"><em>Figure 4: Total Transaction Fees with Halving and ETF Launch Markers</em></p>

---

### Persistence Across Horizons

Cumulative abnormal returns were calculated at days 1-7, 1-14, and 1-30. Price is the only metric where the deviation neither faded nor reversed. Fees spiked early then collapsed, miner revenue decayed toward zero, and transaction activity worsened progressively.

<div align="center">

| Metric | CAR Day 7 | CAR Day 14 | CAR Day 30 |
|--------|-----------|------------|------------|
| BTC price | +20.4% | +19.6% | +19.0% |
| Transaction fees | +63.1% | — | −108.8% |
| Miner revenue | +21.4% | — | +5.0% |
| Transaction count | −15.7% | — | −47.6% |

</div>
<p align="center"><em>Table 4: Cumulative Abnormal Returns Across Event Horizons</em></p>

---

## Conclusion

The election produced a clear and persistent abnormal price response and a mechanically driven miner revenue increase, but neither transaction activity nor fees showed genuine demand-side abnormality. The evidence points to speculative repricing rather than a shift in network economic activity. For an investor, the distinction matters: Bitcoin increasingly responds to political sentiment rather than transactional demand, with direct implications for its classification, valuation, and role in portfolio construction.

Bitcoin was already in a structurally elevated bull cycle driven by the April 2024 halving and the January 2024 ETF launches, and policy uncertainty independently affects Bitcoin volatility (Baker et al., 2020; Ghabri et al., 2022), so isolating the election signal from broader momentum remains difficult. A volatility-adjusted cross-asset framework (Ampountolas, 2025) is the natural extension of this analysis.

---

## References

- Aalborg, H.A., et al. (2023). The relationship between Bitcoin price, network activity, and market behaviour.
- Ampountolas, A. (2025). Volatility-adjusted cross-asset frameworks for cryptocurrency event studies.
- Amsden, R. (2024). Cryptocurrency policy and the 2024 US presidential campaign.
- Baker, S., Bloom, N. and Davis, S. (2020). Policy uncertainty and asset price volatility.
- Cahill, D., et al. (2024). ETF flows, derivatives trading, and Bitcoin price formation.
- Easley, D., O'Hara, M. and Basu, S. (2019). From mining to markets: The evolution of Bitcoin transaction fees. *Journal of Financial Economics*, 134(1), pp.91–109.
- Fama, E., Fisher, L., Jensen, M. and Roll, R. (1969). The adjustment of stock prices to new information. *International Economic Review*, 10(1), pp.1–21.
- Ghabri, Y., et al. (2022). Economic policy uncertainty and Bitcoin volatility.
- MacKinlay, A.C. (1997). Event studies in economics and finance. *Journal of Economic Literature*, 35(1), pp.13–39.
- Tan, X. and Wong, K. (2025). Persistent price effects following the 2024 US election result.

---

<p align="center">
  <a href="election_event_study.ipynb">
    <img src="https://img.shields.io/badge/View%20Notebook-F37626?style=flat&logo=jupyter&logoColor=white"/>
  </a>
</p>

<p align="center">
  <a href="#top">Back to Top ↑</a>
</p>
