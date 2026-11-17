# LinkAja Syariah Feasibility: Monte Carlo Simulation of "Nava by LinkAja"

JBCC 2026 semifinal, client LinkAja. Solution: **Nava by LinkAja**, #TheOnlyMuslimAppYouNeed.
Author: Fausta Yassar Putra Prakoso, FEB UI Management.

This repository holds the Monte Carlo model behind the feasibility section of the proposal. It simulates 10,000 possible worlds (seed 2026) from January 2027 to December 2032 in monthly steps, first without the solution (baseline, "do nothing"), then with the three Nava pillars applied to the same random worlds.

> Every figure is the output of a model built on stated assumptions. It is not a prediction and not an official LinkAja number. Syariah MAU is not published, so the percentage uplifts are more defensible than the absolute MAU levels.

## Headline results (median, with P10 to P90)

| Item | Do nothing | With Nava |
|---|---|---|
| MAU 2032 (million, yearly average) | 1.08 | 1.55 |
| MAU uplift vs do nothing, 2032 | n/a | +39% (P10 19%, P90 56%) |
| MAU growth per year, Dec 2026 to 2032 (CAGR) | 4.3% (2.4 to 6.4) | 10.7% (7.8 to 13.6) |
| Total MAU growth, 2032 vs Dec 2026 | +26% | +75% (51 to 101) |
| Index 2032 (Dec 2026 = 100) | 126 (114 to 141) | 175 (151 to 201) |
| Solution above do nothing in the same simulation | n/a | 100% |

The "+39%" is an increment over do nothing, not Nava's own growth. Nava itself grows about 10.7% per year.

Probability of success under execution risk (each pillar fails completely with probability f, headline f = 30%):

| Design | Above do nothing | Uplift >= 10% | Uplift >= 20% | Uplift >= 30% | Above do-nothing P90 | Target at 80% confidence |
|---|---|---|---|---|---|---|
| Without mitigation | 90% | 72% | 56% | 38% | 46% | +7.2% |
| With mitigation | 98% | 86% | 73% | 57% | 58% | +14.1% |

## Repository structure

```
notebook/simulation_linkaja_syariah.ipynb   executed notebook, all code and charts in order
src/baseline_montecarlo.py                  baseline (do nothing) engine
src/simulation_solution.py                  three-pillar solution engine, risk and growth analysis
output/baseline/                            baseline tables (CSV) and charts (PNG)
output/solution/                            solution tables (CSV) and charts (PNG)
requirements.txt
```

## Part 1. Baseline (do nothing)

- **Users.** Registered users R and monthly active users (MAU) follow a Bass (1969) S-curve with decaying growth.
- **Randomness.** AR(1) macro factor with Student t(5) shocks, a 3% per year recession probability, Poisson shocks (Merton, 1976), Ramadan seasonality, triangular input ranges, and an MAU cap of 0.35 times registered users.
- **Initial state.** Registered users R0 ~ triangular(9.5, 10.0, 10.8) million. Active ratio a0 ~ triangular(5%, 8%, 12.5%). Initial MAU is therefore P5 / P50 / P95 = 0.61 / 0.85 / 1.13 million.
- **Weakest assumption.** a0 is not documented publicly. Do not claim absolute MAU levels; ask LinkAja for Syariah MAU.
- **Public anchors.** Registered users: 1.6M (end 2020), 5.8M (Nov 2021), 7.5M (Jul 2022), more than 9M (Aug 2026), about 10M (Nov 2026).

Baseline growth per year (CAGR, Dec 2026 to 2032, defined as (MAU 2032 annual average / MAU0)^(1/5.5) minus 1):

| Case | CAGR | Total growth |
|---|---|---|
| Bearish (P5) | 1.8% | +11% |
| Base (P50) | 4.3% | +26% |
| Bullish (P95) | 7.1% | +46% |

With Nava: 7.1% / 10.7% / 14.4% per year (total +46% / +75% / +110%).

## Part 2. Solution model (three pillars)

Each pillar is a range (minimum, most likely, maximum), drawn once per world. Effects build up gradually, r_i(t) = 1 - exp(-t / tau_i).

| Parameter | Meaning | Min / mode / max |
|---|---|---|
| M1 | Active-ratio multiplier from the Nava app (pillar 1) | 1.05 / 1.20 / 1.45 |
| U2 | Extra registered users from KOL, mosque QRIS, festivals (pillar 2) | 0.03 / 0.09 / 0.20 |
| U1 | Extra registered users from the app (pillar 1), set to zero to stay conservative | 0 / 0 / 0 |
| q | Activity of new users relative to average | 0.5 / 0.8 / 1.0 |
| M3 | MAU multiplier from retention (pillar 3) | 1.02 / 1.08 / 1.18 |
| G3 | Extra MAU growth per year from **compounding retention** (assumption, no data) | 0 / 0.01 / 0.025 |
| omega | Overlap discount between pillars | uniform 0.7 to 1.0 |
| tau1, tau2, tau3 | Months to build up the effect | U(8,18), U(6,15), U(12,24) |

```
f1 = 1 + (M1 - 1) r1
f2 = 1 + q (U2 r2 + U1 r1)
f3 = (1 + (M3 - 1) r3) (1 + G3)^(r3 t / 12)
uplift L = omega (f1 f2 f3 - 1)               (omega = 1 if only one pillar runs)
MAU_solution = min( MAU (1 + L),  max(12.5%, MAU/R, capped at 35%) x R (1 + U2 r2 + U1 r1) )
```

**Active-ratio cap of 12.5%.** It comes from one comparator, Muslim Pro (5M active of 40M registered, Bisnis.com 2025). It binds in about 49% of simulations in 2032. It is a team assumption, not a ceiling from data.

**Compounding retention (G3).** Added so that Nava's growth is not just a one-off level shift. It is an explicitly labelled assumption with no data behind it. Without G3 the median uplift would be about 34%.

Paired runs (common random numbers) mean the solution-minus-baseline difference is not random luck. Solution effects use seed 99, execution risk uses seed 777.

## Results

**Yearly growth of the median, each year over the previous year.**

| | 2027 | 2028 | 2029 | 2030 | 2031 | 2032 |
|---|---|---|---|---|---|---|
| Do nothing | 3.8% | 6.1% | 4.9% | 3.9% | 3.3% | 2.8% |
| With Nava | 17.5% | 19.1% | 9.5% | 5.6% | 3.8% | 2.9% |

Nava's growth is front-loaded because pillars 1 and 2 are mostly one-off jumps in level; by 2032 the gap in yearly growth is almost gone. Registered users rise from 12.2M to 13.5M (+10.5%) and the 2032 active ratio from 9.4% to 12.4%.

**Pillar contribution (each pillar alone, 2032 uplift median).** Pillar 1 19.5% (7.5 to 31.9), pillar 2 7.9% (4.6 to 12.3), pillar 3 14.7% (6.9 to 21.4), combined 39.4%.

**Sensitivity (Spearman).** M1 0.39, U2 0.24, omega 0.22, G3 0.17, M3 0.16, q 0.06. These are the assumptions most worth validating with client data.

**Audit of seemingly conservative assumptions (median uplift, reference 39.7%).** Cap at 15%: 44.1. No cap: 45.6. No overlap discount: 44.8. Cross-sell U1 of 0 / 2 / 5%: 41.7. All three together: 56.5. None has supporting data, so the reference values are kept.

**Cost break-even.** The solution adds about 1.8 million user-years over 2027 to 2032 (P10 to P90: 1.2 to 2.5). At an illustrative cost of Rp25 billion, it needs about Rp14 thousand of net revenue per extra MAU per year to break even. The costs are illustrative, not proposal costs.

**Robustness.** The convergence spread across 5 seeds is about 0.5%. The probability that the solution exceeds the do-nothing P90 is about 70%.

## Probability of success

Each pillar fails completely with probability f (grid 0, 10, 20, 30, 40%; headline 30%). Pillar 3 requires pillar 1. Pillar effects are **not** tuned to reach a target; f and the mitigation parameters are team assumptions. The reference range for launch failure, about 25% to 45%, comes from weak secondary sources on consumer products.

Mitigation design (valid only if it is in the implementation plan):

- Pillar 1: staged pilot with one relaunch. The relaunch fails again with probability 50%, and effects are delayed by 9 months.
- Pillars 2 and 3: three parallel channels or features. A pillar fails only if all three fail, plus a 10% common-cause probability.

Median uplift by f (without mitigation): 39.4 (f=0), 34.8 (10), 29.5 (20), 24.0 (30), 16.8 (40). With mitigation: 34.7 (20), 33.1 (30), 30.8 (40). At f = 30% with mitigation, the pillars that actually run are: all three 66% (median uplift 39%), only pillar 2 12% (8%), none 2%.

Limitation: failure is binary and mostly independent. In reality pillars can fail partially and be correlated, so real outcomes will be more volatile.

## Safe wording for the proposal

- "Simulation of 10,000 scenarios indicates a median MAU uplift of about 39% over do nothing by 2032 (80% interval 19% to 56%), equal to MAU growth of about 10.7% per year versus 4.3%."
- "With staged-pilot and parallel-channel mitigation and a 30% initial pillar failure assumption, the solution can be promised to deliver at least about 14% uplift with 80% confidence."
- Avoid absolute MAU claims, and state that effect sizes are team assumptions to be validated.

## Items to verify before submission

1. Syariah MAU and the starting active ratio a0 (ask LinkAja).
2. The 12.5% active-ratio cap rests on one comparator.
3. G3 (compounding retention) has no data.
4. Failure probability f and the three mitigation parameters are team assumptions.
5. Historical registered-user path before 2027 is a regression on five public points.
6. U1 is set to zero, and the survey behind some inputs has n = 50.
7. DMI claim, launch timeline, budget and ARPU need client confirmation.
8. The 9M versus 10M registered-user discrepancy between sources.
9. Davis (1989) was cited from memory and not re-verified.

## How to run

```
pip install -r requirements.txt
python src/baseline_montecarlo.py                     # writes output/baseline/
python src/simulation_solution.py --test              # writes output/solution/, plus 5-seed convergence test
```

Options: `--n 10000 --seed 2026 --outdir <folder>`. The notebook runs top to bottom with no other file: open `notebook/simulation_linkaja_syariah.ipynb` locally or upload it to Google Colab.

## References

Bass (1969) new-product growth model. Metropolis and Ulam (1949), Monte Carlo method. Merton (1976), jump-diffusion. Malcolm et al. (1959), triangular/PERT estimates. Saltelli et al. (2008), global sensitivity analysis. Hamari, Koivisto and Sarsa (2014), gamification. Henderson (1970), seasonal adjustment. Success-rate references: JPIM (via Oregon State PACE blog), McTague (2023), Tenet (2026).
