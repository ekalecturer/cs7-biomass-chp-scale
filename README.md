# The Feasibility Stress Test — Biomass Gasification CHP, Three Sizes (Case Study #7)

Part of [The Feasibility Stress Test](https://github.com/ekalecturer) — real Monte Carlo and financial-engineering case studies for capex decisions.

## The case

A biomass gasification cogeneration (CHP) plant, bubbling fluidized bed gasifier feeding an internal combustion engine, modeled at three capacities by the same research team: 13.6 kWe, 136 kWe, and 1,940 kWe. Same 20-year discounted cash flow structure, same 6% discount rate, same eight-variable Monte Carlo risk model, applied consistently across all three sizes. Each size went through a deterministic cash flow first, then 1,000 Monte Carlo trials.

At the two smaller sizes, the Monte Carlo mean NPV landed *above* the deterministic point estimate (the base case undersold the project). At the largest size, the Monte Carlo mean landed *below* the deterministic estimate (the base case oversold it). Same methodology, same team, opposite direction of error, just by changing plant capacity.

## Source

Colantoni, A., Villarini, M., Monarca, D., Carlini, M., Mosconi, E.M., Bocci, E., Rajabi Hamedani, S. (2021). Economic analysis and risk assessment of biomass gasification CHP systems of different sizes through Monte Carlo simulation. *Energy Reports*, 7, 1954-1961. https://doi.org/10.1016/j.egyr.2021.03.028

Gold open access, Creative Commons BY-NC-ND, Elsevier. The full six-page paper was read directly on ScienceDirect, not just the abstract. Every confirmed number in this repo is transcribed from the paper's own Tables 3-6 and Section 4 text.

## Confirmed findings

| Plant size | Deterministic NPV | MC mean NPV (1,000 trials) | Gap | P(NPV > 0) | Coefficient of variation |
|---|---|---|---|---|---|
| 13.6 kWe | €33,900 | €64,981 | +91.7% | 66% | 0.679 |
| 136 kWe | €537,070 | €847,980 | +57.9% | 90% | 0.588 |
| 1,940 kWe | €13,268,960 | €8,969,213 | -32.4% | 90% | 0.536 |

Biomass cost is the single most damaging variable to NPV at every size (66% to 85% of operating cost), but the second-ranked driver changes by scale: electricity price at the smallest plant, working hours and gasification efficiency (syngas yield) at the largest.

## What's in this repo

- `notebook.ipynb` — Python reconstruction of all three plant sizes' deterministic cash flows and 8-variable Monte Carlo models (triangular distributions built from the paper's Table 5 min/base/max ranges), executed end to end for 10,000 trials per size.
- `data/confirmed_facts.csv` — every real number transcribed from the paper, all three sizes.
- `data/input_distributions.csv` — the paper's eight input variables and their min/base/max ranges, all three sizes.
- `data/notebook_reconstruction_output.json` — this reconstruction's own simulated output, for direct comparison against the paper's real Table 6 figures.
- `docs/index.html` — an interactive browser-based simulator: pick a plant size, adjust six key inputs, click Run, see your own NPV distribution.
- `docs/notebook.html` — a rendered HTML export of the notebook.

## Honesty disclosure

The paper publishes min/base/max ranges for all eight variables (Table 5) but does not state which probability distribution shape it assigned to each one inside its Crystal Ball-style model, any correlation between inputs, or its random seed. This reconstruction uses triangular distributions throughout, sampled independently in the notebook (with an illustrative Cholesky-based Gaussian copula between investment cost and biomass cost added only in the browser simulator, as a disclosed modeling choice, not something stated in the paper).

**The reconstruction does not fully match the paper, and that gap is stated directly rather than minimized.** At the two smaller sizes, the notebook's Monte Carlo mean lands above the deterministic case, the same direction as the paper, though at a larger magnitude than the paper's own +91.7% and +57.9% gaps. At the largest size, the notebook's reconstruction produces a Monte Carlo mean *above* the deterministic case, while the paper reports the opposite: a mean 32.4% *below* the deterministic figure. The published Table 5 ranges alone were not sufficient to reproduce that reversal; it likely depends on input correlations, a non-triangular distribution shape, or a cost/efficiency interaction specific to the largest plant's design that the paper does not publish in enough detail to reconstruct exactly. The notebook's own printed output states this gap directly, size by size, rather than claiming a match that isn't there.

## Run it yourself

```bash
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

Or open `docs/index.html` directly in any browser — the simulator runs entirely client-side in JavaScript, no server or install required.

## License

MIT. See `LICENSE`.

---

Part of **The Feasibility Stress Test** — real Monte Carlo and financial-engineering case studies for capex decisions.
