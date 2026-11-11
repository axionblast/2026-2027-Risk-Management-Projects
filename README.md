# CPPI Portfolio Insurance — Risk-Constrained Leverage

A short quantitative risk modeling project based on **KTH SF2980 Risk Management, Session 2, Problem 5**.

## Objective

How much exposure to a risky asset can a Constant Proportion Portfolio Insurance (CPPI) strategy take while respecting different downside-risk limits? Are these decisions robust to a rise in market volatility?

## Model

One-period CPPI with risky gross return $R=\exp(\mu+\sigma Z)$, $Z\sim N(0,1)$, and zero return on cash:

$$V_1=V_0+\alpha(V_0-G)(R-1).$$

- $G$: portfolio floor (the minimum desired future portfolio value, **not** the cash balance)
- $V_0-G$: cushion (capital above the floor)
- $\alpha$: leverage multiplier applied to the cushion
- Baseline $\mu=0.05$, $\sigma=0.20$, $p=0.01$
- Illustrative assumptions: $V_0=\$100$, $G=\$80$, $\gamma=0.10$

## Methods and results

| Risk rule | Maximum leverage | Risky allocation* | Cash allocation* |
|---|---:|---:|---:|
| (a) Floor-breach probability $\leq 1\%$ | **2.9426×** | $58.85 | $41.15 |
| (b) ES constraint, $\gamma=0.10$ | **2.8800×** | $57.60 | $42.40 |

*Dollar allocations are for the illustrative $100 portfolio and $80 floor; they are not given in the original exercise.*

For (b), the ES condition is equivalent to requiring that the **average portfolio value in the worst 1% of outcomes is at least $78**. This is the $80 floor minus 10% of the $20 cushion. Importantly, the worst 1% of outcomes are **not defined** as the events in which the floor is breached.

![Baseline leverage](figures/leverage_comparison.png)

## Volatility stress test

Keeping each leverage multiplier fixed at its baseline-calibrated value, we increase volatility from 20% to 30% and 40% while keeping $\mu$ fixed.

![Volatility stress test](figures/stress_test.png)

| $\sigma$ | Strategy (a): floor breach | Strategy (b): floor breach | Strategy (a): mean wealth, worst 1% | Strategy (b): mean wealth, worst 1% |
|---|---:|---:|---:|---:|
| 0.20 | 1.00% | 0.86% | $77.52 | $78.00 |
| 0.30 | 6.05% | 5.61% | $69.08 | $69.73 |
| 0.40 | 12.24% | 11.68% | $62.61 | $63.40 |

**Finding:** The baseline-approved multipliers do not remain safe under higher volatility. An ES constraint limits average lower-tail loss, while the probability constraint limits the frequency of breaching the floor. Neither rule guarantees protection when the assumed return distribution changes.

## Run locally

```bash
pip install -r requirements.txt
jupyter notebook CPPI_Risk_Analysis.ipynb
```

Run all cells from the project folder. The notebook reproduces the numerical results and saves the figures under `figures/`.

## Scope / limitations

- One-period, lognormal gross returns; not calibrated to market data
- Fixed floor and zero interest on cash
- Analytic parameter stress test, **not** a historical backtest or Monte Carlo simulation
- Ignores transaction costs, jumps, slippage, and multi-period CPPI rebalancing

**Educational modeling project, not financial advice.**
