# README: Monte Carlo Simulation & Risk Assessment of Cab Operations (January 2026)

## Project Overview

This project implements a **historical-replay Monte Carlo simulation model** to evaluate the financial and liquidity performance of a cab business operating through January 2026 under daily operating conditions derived from 2025.

The model answers two central business questions:

1. *How much operating surplus might we earn if we operate under 2025-like conditions for another month?*
2. *How much opening cash reserve would protect the business from running out of money during the month?*

---

## Repository Structure & File Manifest

* **`cab_daily_history.csv`**: Historical city-wide daily operating records (1 Jan 2023 – 31 Dec 2025). The reference period used is 1 January–31 December 2025.
* **`cab_zone_shift_history.csv`**: Granular zone-and-shift historical records for operational pattern analysis.
* **`ride_requests.csv`**: Individual raw trip/request logs.
* **`daily_context.csv`**: Additional daily contextual features (weather, weekday numbers, special events).
* **`future_january_2026_plan.csv`**: Planned operational inputs for January 2026 (used solely to verify forecast dates in this baseline model).
* **`future_cash_calendar.csv`**: January 2026 fixed-cost allocations, fixed cash payment schedules, and known opening receivables.
* **`scenario_assumptions.csv`**: Scenario definitions (evaluated via dedicated fuel-cost stress tests).
* **`DATA_DICTIONARY.csv`**: Full metadata reference defining row grains, unique keys, units, and column definitions.
* **`INSTRUCTOR_monthly_controls.csv`**: Instructor reference benchmarks for monthly validation.
* **`eligible_block_starts.csv`**: Saved starting dates for valid 7-day blocks starting on Thursdays in 2025 ($B = 52$).
* **`simulation_indices.csv`**: Saved $(10000 \times 5)$ int64 simulation index matrix generated using `PCG64(20261102)`.
* **`baseline_simulation_results.csv`**: Full path-level outputs for all 10,000 baseline simulation runs.
* **`scenario_comparison_results.csv`**: Summary metrics comparing baseline against fuel stress tests (+10% and +20%).
* **`demand_backtest_results.csv`**: 12-month rolling backtest results for total request volume across 2025.
* **`model_assumptions_record.txt`**: Plain-text record of model metadata, seeds, versions, and core assumptions.
* **`monte_carlo_simulation_plots.png`**: Multi-panel visualization of distributions, cash trajectories, and scenario risks.

---

## Key Model Specifications & Reproducibility Rules

* **Forecast Period:** 1–31 January 2026 (Forecast origin: 31 December 2025).
* **Simulation Size:** 10,000 paths.
* **Random Number Generator:** NumPy Generator with PCG64 (`seed = 20261102`), generating a single $(10000 \times 5)$ simulation index matrix.
* **Sampling Rule:** Each 31-day month is constructed by sampling 5 seven-consecutive-day blocks starting on Thursdays from 2025 with replacement, concatenating them, and truncating to the first 31 days.
* **Financial Mechanics:**
* **Operating Surplus:** $\text{Net Revenue} - \text{Variable Expenses} - \text{Fixed Cost Allocation}$.
* **Working Capital Lag:** Digital sales settle after a 2-calendar-day lag; cash sales settle same-day.
* **Required Opening Cash ($R_i$):** $\max(0, -\text{minimum cumulative cash flow over the path})$.



---

## Summary of Key Findings

1. **Baseline Operating Surplus:** Across 10,000 simulated paths, the mean monthly operating surplus is **₹622,496.80** (Median P50: **₹624,064.46**; Central 90% interval P05–P95: **₹354,786.78 to ₹887,164.38**).
2. **Loss vs. Shortage Risk:**
* **Monthly Operating Loss Probability:** **0.00%** (minimum surplus across all paths was +₹96,336.89).
* **Cash Shortage Probability:** **0.00%** assuming an opening cash balance of **₹1,000,000.00**.


3. **Opening Cash Reserve:** The 95th percentile required opening cash reserve ($R_{95}$) is **₹730,834.07**.
4. **Fuel Stress Tests:**
* **+10% Fuel Cost:** Mean surplus drops to **₹377,762.56** (Loss probability: **0.45%**).
* **+20% Fuel Cost:** Mean surplus drops to **₹133,028.32** (Loss probability: **19.54%**).



---

## How to Run in Google Colab

1. Upload all required CSV files (`cab_daily_history.csv`, `future_cash_calendar.csv`, etc.) into your Colab runtime environment.
2. Verify environment packages: Python `3.10+`, Pandas `2.0+`, NumPy `1.26+`.
3. Execute the simulation steps sequentially as outlined in the classroom specification.
