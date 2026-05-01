# bwci-thesis

Behavioral Work Continuity Index: Development of a Measure and Comparison of Machine Learning Models

This repository contains materials and code for a master's thesis on the Behavioral Work Continuity Index (BWCI): a proxy measure of work continuity based on behavioral telemetry from computer usage.

The project uses the BEHACOM dataset as the main source of keyboard, mouse, application usage, and system telemetry aggregated in one-minute windows.

## Local setup

Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Quick import check:

```bash
python -c "import pandas, numpy, sklearn"
```

## Data

BEHACOM data should be stored locally under `data/`.

The repository does not commit raw data, processed datasets, virtual environments, generated outputs, or local caches.

## External references

- BEHACOM dataset: https://data.mendeley.com/datasets/cg4br62535/2
- BEHACOM article: https://www.sciencedirect.com/science/article/pii/S2352340920306612
