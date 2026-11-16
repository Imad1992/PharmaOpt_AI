# PharmaOpt AI

**A reproducible pharmaceutical demand forecasting and procurement optimisation portfolio prototype.**



## What it does

- Generates 240 days of reproducible synthetic demand for five anonymised medicine products.
- Forecasts daily demand using weekday-conditioned empirical bootstrapping, reporting mean and 10th/90th percentiles.
- Evaluates forecasts on a chronological 14-day holdout with MAE and empirical interval coverage.
- Uses SciPy mixed-integer linear programming to recommend whole supplier packs, respecting a global purchasing budget and maximum order packs.
- Penalises unmet forecast demand and incorporates a basic supplier reliability cost adjustment.
- Presents results in a Streamlit dashboard with interactive scenarios, plots, KPIs and CSV export.

## Quick start

Requires Python 3.10+.

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -e .
streamlit run app.py
```

Open the local URL printed by Streamlit (usually http://localhost:8501).

## Run tests

```bash
python -m pip install -e . pytest  # alternatively: pip install -e . pytest
pytest -q
```

If the optional `dev` extra is unavailable, use `pip install -e . pytest`.

## Architecture

```text
app.py                          Streamlit UI
src/pharmaopt/core.py           Synthetic data, forecasts, MILP, backtesting
tests/test_core.py              Reproducibility and constraint checks
.github/workflows/tests.yml     GitHub Actions CI
```

## Methodology

**Demand model:** historical units are grouped by weekday; bootstrap resampling estimates future-day expected demand and empirical predictive intervals. This is a simple interpretable baseline, not a calibrated Bayesian model.

**Optimisation:** choose nonnegative integer pack quantities and nonnegative shortage slack. Minimise total purchasing cost (with supplier risk adjustment) plus weighted shortage penalties, subject to a budget, pack limits, and demand-coverage constraints. Conservative mode sums each future day's p90 value; this is *not* a joint 90% service-level guarantee.

**Evaluation:** train only on dates before a 14-day holdout, predict the holdout and compute mean absolute error and empirical interval coverage. No random shuffle of future observations into training.

## Roadmap (not implemented)

1. Replace bootstrap with hierarchical Bayesian longitudinal forecasting, posterior predictive checks and calibration.
2. Add multiple suppliers per SKU, lead-time distributions, reorder points, expiry dates, licensing and batch-level constraints.
3. Evaluate rolling-origin backtests, cost-versus-service tradeoffs and sensitivity analysis.
4. Introduce validated ERP adapters, authentication, audit trails, approval workflow and compliance review.
5. Validate against permissioned, de-identified operational datasets and domain expert feedback.

## Responsible use

Synthetic demonstration only. Medicine sourcing, licensing, regulatory compliance, patient safety and real purchasing require validated data, appropriate professional oversight and human approval. The model does not make clinical recommendations.



**PharmaOpt AI — Pharmaceutical Procurement Forecasting & Optimisation (Python, SciPy, Streamlit)**
Built a synthetic-data decision-support prototype combining time-series demand forecasts, uncertainty intervals and mixed-integer purchasing optimisation with budget and supplier constraints; developed an interactive dashboard, chronological backtesting and automated tests.

## License

MIT; see LICENSE.
