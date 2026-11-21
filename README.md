# Classifying Palmer penguin species from body measurements

**Authors:** Ariana Cruz-Montgomery, Taishi Huang, and Zaakeer Ahmad  

## Summary

Can bill length, bill depth, flipper length, and body mass distinguish Adelie, Chinstrap, and Gentoo Penguins? We investigated this question using the Palmer penguins dataset and multinomial logistic regression. The selected model correctly classified all 85 complete observations in the held-out test set, with accuracy and macro-F1 of 1.00. These results suggest that body measurements provide useful information for identifying species.

## Report

The final analysis report can be found [here](https://ubc-mds-2026-27.github.io/DSCI-522-GROUP-29-Classifying-Palmer-penguin-species-from-body-measurements/analysis/penguin-species-classification.html)

## How to run the analysis

### 1. Clone the repository

Using terminal navigate to the folder where you want to keep the project, then run:

```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-GROUP-29-Classifying-Palmer-penguin-species-from-body-measurements.git penguin-project
cd penguin-project
```

### 2. Set up the environment

```bash
uv sync --locked
```

Run the above command from the root of the repository to create a `.venv` environment and install the versions recorded in `uv.lock`.

### 3. Render the report

From the repository root, point Quarto to the project environment and run the analysis:

```bash
export QUARTO_PYTHON="$PWD/.venv/bin/python"
uv run quarto render analysis/penguin-species-classification.qmd
```

### 4. View the results

Open `analysis/penguin-species-classification.html` in a browser to view the final report.

## Dependencies

Install the following tools before running the analysis:

- [Git](https://git-scm.com/install/), to clone the repository
- [uv](https://docs.astral.sh/uv/getting-started/installation/), to manage Python and the project environment
- [Quarto](https://quarto.org/docs/get-started/), to execute and render the report

The project uses Python 3.12. Python dependencies, including pandas, matplotlib, scikit-learn, and the Jupyter execution packages, are listed in [`pyproject.toml`](pyproject.toml). Resolved dependency versions are recorded in [`uv.lock`](uv.lock).

## Licenses

The **analysis report** is licensed under [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)](https://creativecommons.org/licenses/by-nc-nd/4.0/). See the [license file](LICENSE.md) for more information.

The **software code** is licensed under the **MIT License**. See the [license file](LICENSE.md) for more information.

The original penguin data are available under **CC0**, as described in the palmerpenguins documentation. Their licence is separate from the licences for our report and code.

## AI use statement

Cursor was used to review the code and to help revise the summary and the discussion. It also checked the grammar throughout the report. An example prompt was: "Review the Quarto report for grammar, and revise the summary and the discussion so that the claims match the training-set figures and the single test-set evaluation."

## Acknowledgements

The data were collected by Kristen Gorman and the Palmer Station Long Term Ecological Research program, and distributed in a simplified form through palmerpenguins by Allison Horst, Alison Hill, and Kristen Gorman. The full report includes the references supporting the data source and statistical methods.
