# AGENTS.md

## Project Context

This repository supports a master's thesis titled:

**Behavioral Work Continuity Index: Development of a Measure and Comparison of Machine Learning Models**

The thesis develops and evaluates the Behavioral Work Continuity Index (**BWCI**):
a proxy measure of work continuity based on behavioral telemetry from computer usage.

The main dataset is **BEHACOM**, stored locally under `data/BEHACOM/Behacom`.
It contains one-minute aggregated telemetry for keyboard activity, mouse activity,
application usage, and basic system metrics.

## Methodological Ground Rules

- The thesis is about **behavioral work continuity** and **risk of work interruption**,
  not direct measurement of focus, attention, concentration, productivity, or mental state.
- BEHACOM has no ground-truth labels for `focused / distracted`; any target variable must be treated as a **proxy**.
- Avoid claims that BWCI measures psychological focus. Use wording such as:
  - behavioral continuity,
  - work continuity,
  - work disruption risk,
  - observable behavioral proxy.
- The main methodological risk is circularity: do not present prediction of BWCI from the same contemporaneous features
  used to construct BWCI as strong evidence. Prefer future prediction, temporal separation, baselines, and sensitivity analysis.
- External validation on own data is planned as a later strengthening step, not part of the initial BEHACOM audit.

## Current Working Decisions

- Use local `venv`; dependency source is a single `requirements.txt`.
- Do not commit raw data, processed datasets, generated outputs, local archives, or `.venv`.
- `docs/*.pdf`, `docs/*.md`, and markdown stage reports are local planning material and are ignored by Git.
- Analytical stage reports should eventually be represented as notebooks, not standalone markdown reports.
- For data exploration, prefer notebooks as the main artifact.
- Add scripts only when a workflow becomes stable and repeatable enough to justify automation.

## Repository Conventions

- Keep implementation minimal until the analysis requires more structure.
- Prefer clear, small additions over premature architecture.
- Put stable reusable logic under `src/bwci/` only after it has emerged from notebook exploration.
- Keep generated outputs under `outputs/`; these are local artifacts and ignored by Git.
- Keep local BEHACOM data under `data/`; this is ignored by Git.
- Do not commit unless the user explicitly asks for a commit.

## Code Style

- Use **two spaces** for Python indentation, not four.
- Keep `max-line-length=150`; `.pylintrc` is the source of truth for lint settings.
- Always use type hints for functions and variables where they improve clarity.
- All or almost all functions should have English docstrings.
- Preferred docstring style includes spaces immediately after opening triple quotes and before closing triple quotes:

```python
def get_active_window_name() -> str:
  """ Get the name of the active window.

    Returns:
      str: The name of the active window.

    Raises:
      NotImplementedError: If the platform is not supported.
  """
```

- Short enum/class/module docstrings may be one line, e.g. `""" Enum for project statuses. """`.
- Comments and docstrings should be written in English.
- Notebooks should follow the same code style in code cells where practical.

## Data Handling Notes

Known BEHACOM facts from initial inspection:

- Local path: `data/BEHACOM/Behacom`
- Files: 12 user CSV files.
- Each CSV has 12,051 columns.
- Total rows before exclusions: 167,058.
- User 2 has only 179 rows and is expected to be excluded from most analyses.
- After excluding User 2, the expected row count is 166,879.
- Digraph columns dominate the schema and should not be loaded casually during early EDA.

Practical implication: avoid full CSV loads unless there is a specific reason.
Prefer selective column loading for early analysis.

## Preferred Work Rhythm

For each thesis stage:

1. Prepare a short plan before implementation.
2. Execute the smallest useful version.
3. Validate outputs.
4. Summarize results briefly.
5. Decide the next step with the user.

For exploratory data analysis:

- Use notebooks for the primary narrative.
- Include markdown cells explaining intent, methodological assumptions, and findings.
- Keep notebook outputs focused: tables, small summaries, and plots that directly support decisions.
- When notebook logic stabilizes, consider extracting reusable parts into `src/bwci/`.

## Useful Skills

Use these skills when the task matches their scope:

- `codebase-onboarding`: when orienting a new agent to the repo structure or updating contributor-facing project notes.
- `senior-data-engineer`: when designing or implementing data loading, audit, ETL, validation, storage layout,
  or performance-sensitive processing.
- `senior-data-scientist`: when defining BWCI, constructing labels, engineering features, comparing ML models,
  designing validation, and interpreting model results.
- `statistical-analyst`: when analyzing sensitivity, stability, correlations, statistical tests, confidence intervals,
  or validation against external/self-report data.
- `senior-qa` or `tdd-guide`: later, when stable pipeline functions need tests.
- `documents`, `presentations`, or `spreadsheets`: later, only if the user asks to create or edit thesis documents,
  defense slides, or spreadsheet summaries.

Do not use skills just to add ceremony. Use them when they materially improve the current step.

## Current Caution

There may be uncommitted local work. Always inspect `git status --short -uall` before editing. Never revert user changes unless explicitly requested.
