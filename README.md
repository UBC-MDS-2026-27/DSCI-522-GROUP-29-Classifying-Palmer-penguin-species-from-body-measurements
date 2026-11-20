# Classifying Palmer penguin species from body measurements

**Authors:** Ariana Cruz-Montgomery, Taishi Huang, and Zaakeer Ahmad

## Project overview

Body measurements vary both within and between penguin species. This project examines whether four measurements—bill length, bill depth, flipper length, and body mass—provide enough information to distinguish Adelie, Chinstrap, and Gentoo penguins in the Palmer Archipelago.

We approach this as a supervised classification problem using multinomial logistic regression. Our goal is to assess the predictive value of these measurements while keeping data exploration and model selection separate from the final test evaluation. The Quarto report includes the Python code, exploratory figures, model-selection results, and a discussion of the findings and their limitations.

## Data and study context

We use the simplified `penguins` table from the [palmerpenguins package](https://allisonhorst.github.io/palmerpenguins/), which contains observations of 344 adult penguins sampled near Palmer Station, Antarctica. The table records sampling years as 2007, 2008, and 2009, corresponding to the 2007/08–2009/10 breeding seasons described in the original study.

| Species | Observations |
| --- | ---: |
| Adelie | 152 |
| Chinstrap | 68 |
| Gentoo | 124 |
| **Total** | **344** |

The response variable is `species`. The predictors are:

| Variable | Measurement | Unit |
| --- | --- | --- |
| `bill_length_mm` | Bill length | mm |
| `bill_depth_mm` | Bill depth | mm |
| `flipper_length_mm` | Flipper length | mm |
| `body_mass_g` | Body mass | g |

Island, sex, and year are excluded from the predictor set to keep the analysis focused on body measurements. Island is strongly associated with species in this sample, while sex has missing values and year represents the sampling season. Two birds are missing all four measurements and are excluded from fitting and evaluation. Birds with missing sex values remain in the analysis because sex is not used by the model.

The report downloads the [source CSV](https://raw.githubusercontent.com/allisonhorst/palmerpenguins/main/inst/extdata/penguins.csv) and writes a local copy to `data/raw/`.

## Analytical approach

We first divide the 344 observations into training and test sets using a stratified 75:25 split with random seed **522**. After removing rows with missing predictors, the analysis uses **257 training observations** and **85 test observations**. Exploratory summaries and plots use the cleaned training set only.

The model combines standardization with L2-penalized multinomial logistic regression in a scikit-learn pipeline. Standardization places the predictors on comparable scales, which matters because body mass is recorded in grams while the other measurements are recorded in millimetres. Keeping this step inside the pipeline ensures that each cross-validation fold uses scaling parameters estimated from its own training observations.

We compare inverse regularization strengths of **C = 0.01, 0.1, 1, 10, and 100** using five-fold stratified cross-validation. Selection is based on mean macro-F1, which gives each species equal weight despite differences in class size. The selected pipeline is then refitted on the full cleaned training set and evaluated on the held-out test set using accuracy, macro-F1, and a confusion matrix.

## Main findings

Cross-validation selected **C = 100**, with a mean validation macro-F1 of **0.986**. The model correctly classified all 85 complete observations in the test set.

| Evaluation measure | Result |
| --- | ---: |
| Mean cross-validation macro-F1 at the selected C | 0.986 |
| Test accuracy | 1.00 |
| Test macro-F1 | 1.00 |
| Correct test predictions | 85 / 85 |

The test set contains 37 Adelie, 17 Chinstrap, and 31 Gentoo penguins. For comparison, a classifier that always predicts Adelie, the most common training class, achieves approximately **0.44 accuracy** and **0.20 macro-F1** on the same test set.

The training summaries show that Gentoo penguins are generally heavier, with longer flippers and shallower bills. Adelie and Chinstrap penguins overlap more in body mass and flipper length, while bill length provides a clearer descriptive distinction between them.

These results indicate strong predictive separation within this dataset. However, the perfect test score should be interpreted in the context of a small test sample drawn from the same study as the training data. The observations cover a limited geographic area and three breeding seasons, and the random split does not establish performance on new colonies or future seasons.

The full analysis is available in [`analysis/penguin-species-classification.html`](analysis/penguin-species-classification.html). Download the file and open it in a browser to view the rendered report.

## Reproducing the analysis

### Requirements

The project uses **Python 3.12**, with dependencies defined in `pyproject.toml` and their resolved versions recorded in `uv.lock`. Install the following tools before running the analysis:

- [Git](https://git-scm.com/downloads) to obtain the repository
- [uv](https://docs.astral.sh/uv/getting-started/installation/) to manage Python and the project environment
- [Quarto](https://quarto.org/docs/get-started/) to execute and render the report

An internet connection is required to install dependencies and download the data during rendering. The commands below are for macOS or Linux.

### 1. Clone the repository

```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-GROUP-29-Classifying-Palmer-penguin-species-from-body-measurements.git penguin-project
cd penguin-project
```

### 2. Install the project dependencies

```bash
uv sync --locked
```

This creates a local `.venv` environment using the versions in `uv.lock`. uv can also install the Python version specified by the project if it is not already available.

### 3. Render the report

Run the following commands from the repository root:

```bash
export QUARTO_PYTHON="$PWD/.venv/bin/python"
uv run quarto render analysis/penguin-species-classification.qmd
```

The rendered report is written to `analysis/penguin-species-classification.html`. Its resources are embedded, allowing the HTML file to be viewed and shared on its own.

Rendering executes the analysis from data download through test evaluation. It also rewrites the local data copy and the processed training and test tables. The required data directories are created automatically.

## Repository structure

```text
analysis/
  penguin-species-classification.qmd    Report source and analysis code
  penguin-species-classification.html   Rendered report
data/
  raw/
    penguins.csv                       Local copy of the source data
  processed/
    penguins_train.csv                 Cleaned training observations
    penguins_test.csv                  Cleaned test observations
.python-version                        Project Python version
pyproject.toml                         Project metadata and dependencies
uv.lock                                Locked dependency versions
LICENSE                                Project licence
README.md                              Project overview and reproduction instructions
```

## Acknowledgements and licensing

The original data were collected by Kristen Gorman and the Palmer Station Long Term Ecological Research program. Allison Horst, Alison Hill, and Kristen Gorman made the simplified dataset available through the palmerpenguins package.

The penguin data are released under **CC0**. The [palmerpenguins documentation](https://allisonhorst.github.io/palmerpenguins/) provides the original data citations and further acknowledgements. The repository includes an [MIT licence](LICENSE).

## References

- Gorman, K. B., Williams, T. D., and Fraser, W. R. (2014). Ecological sexual dimorphism and environmental variability within a community of Antarctic penguins (genus *Pygoscelis*). *PLOS ONE, 9*(3), e90081. https://doi.org/10.1371/journal.pone.0090081
- Horst, A. M., Hill, A. P., and Gorman, K. B. (2020). *palmerpenguins: Palmer Archipelago (Antarctica) penguin data*. R package version 0.1.0. https://doi.org/10.5281/zenodo.3960218
- James, G., Witten, D., Hastie, T., and Tibshirani, R. (2021). *An introduction to statistical learning, with applications in R* (2nd ed.). Springer. [Book website](https://www.statlearning.com/)
- Pedregosa, F., et al. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research, 12*, 2825–2830. [Article](https://jmlr.org/papers/v12/pedregosa11a.html)
