# Classifying Palmer penguin species from body measurements

**Authors:** Ariana Cruz-Montgomery, Taishi Huang, and Zaakeer Ahmad

## Summary

Can bill length, bill depth, flipper length, and body mass distinguish Adelie, Chinstrap, and Gentoo Penguins? We investigated this question using the Palmer penguins dataset and multinomial logistic regression. The selected model correctly classified all 85 complete observations in the held-out test set, with accuracy and macro-F1 of 1.00. These results suggest that body measurements provide useful information for identifying species.

## Report

The final report is published at <https://ubc-mds-2026-27.github.io/DSCI-522-GROUP-29-Classifying-Palmer-penguin-species-from-body-measurements/>.

## Usage

The analysis runs in a Docker container, so everyone uses the same software and package versions. The image is published on Docker Hub at [zaakeer/dsci-522-group-29-classifying-palmer-penguin-species-from-body-measurements](https://hub.docker.com/r/zaakeer/dsci-522-group-29-classifying-palmer-penguin-species-from-body-measurements).

### Setup

Install and launch [Docker](https://www.docker.com/get-started/). Then clone this repository and move into it:

```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-GROUP-29-Classifying-Palmer-penguin-species-from-body-measurements.git penguin-project
cd penguin-project
```

Run all the commands below from the root of this repository.

### 1. Make an ssh key

If you do not have an ssh key yet, make one and press Enter at each question:

```bash
ssh-keygen -t ed25519
```

The container uses this key to let you log in. If you skip this step, Docker creates a folder named `~/.ssh/id_ed25519.pub`, and connecting to the container over ssh fails until you delete that folder and make the key.

### 2. Download the image

```bash
docker compose pull
```

### 3. Render the report

```bash
docker compose run --rm analysis-env uv run quarto render analysis/penguin-species-classification.qmd
```

Rendering runs the analysis from data download through the test evaluation, so it needs an internet connection. It rewrites the files in `data/raw/` and `data/processed/`. The rendered report is written to `analysis/penguin-species-classification.html`. Because of `--rm`, the container is removed by itself when the render finishes, so there is nothing to stop afterwards.

## Contributing

To change the analysis, add a package, or update the published report, see [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Dependencies

- [Docker](https://www.docker.com/)
- The image is built from the [`Dockerfile`](Dockerfile) in this repository. It installs Quarto, uv, and Python with the packages listed in [`pyproject.toml`](pyproject.toml). Exact versions are locked in [`uv.lock`](uv.lock).

## Licenses

The analysis report is licensed under [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)](https://creativecommons.org/licenses/by-nc-nd/4.0/). See the [license file](LICENSE.md) for more information.

The software code is licensed under the MIT License. See the [license file](LICENSE.md) for more information.

The original penguin data are available under CC0, as described in the palmerpenguins documentation. Their licence is separate from the licences for our report and code.

## AI use statement

Cursor was used to review the code and to help revise the summary and the discussion. It also checked the grammar throughout the report. An example prompt was: "Review the Quarto report for grammar, and revise the summary and the discussion so that the claims match the training-set figures and the single test-set evaluation."

Cursor also drafted these container instructions for the README and CONTRIBUTING. An example prompt was: "Draft the Milestone 2 README and CONTRIBUTING sections so a reader can run the analysis in the Docker container and a contributor can work in it with Positron."

## Acknowledgements

The data were collected by Kristen Gorman and the Palmer Station Long Term Ecological Research program, and distributed in a simplified form through palmerpenguins by Allison Horst, Alison Hill, and Kristen Gorman.
