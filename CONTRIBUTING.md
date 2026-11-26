# Contributing

This describes how to propose a change to the Palmer penguin species project.

## Fixing typos

Small typos in documentation may be edited in the source file.

- Edit the Quarto source file `analysis/penguin-species-classification.qmd`.
- Do not edit the rendered file `analysis/penguin-species-classification.html` by hand.

## Prerequisites

Before you make a substantial pull request, open an issue and make sure someone from the team agrees that it should be done.

For a feature, describe the feature, its benefit, and how it differs from the current project.

For a bug, describe the current behaviour, why it is a problem, and the behaviour you expect.

## Setting up the project

Development uses the same Docker container that runs the analysis. Follow the [Usage section of the README](README.md#usage) first: install Docker, clone the repository, make an ssh key, and run `docker compose pull`. Run the commands below from the root of this repository.

## Working in the container

Edit and run the code inside the container by connecting Positron to it over ssh. The project folder is shared with the container at `/workspace`, so a file you change there also changes on your computer.

Once, before your first connection:

1. Install [Positron](https://positron.posit.co/download.html). Its Remote SSH feature is built in.
2. Confirm that you followed the [Usage section of the README](README.md#usage): `~/.ssh/id_ed25519.pub` exists, and you have run `docker compose pull`.
3. Check that Git knows who you are:

```bash
git config --global user.email
```

It should print the email of your GitHub account. If it prints nothing, set your name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

`docker-compose.yml` shares your `~/.gitconfig` with the container, so commits you make there count as yours.

Each time you work on the project:

1. Start the container in the background:

```bash
docker compose up -d
```

`docker compose ps` should list `analysis-env` as running.

2. In Positron, open the Command Palette with `Ctrl+Shift+P` on Windows or `Cmd+Shift+P` on Mac.
3. Run **Remote SSH: Show Remote Menu**, then choose **Connect to Host**.
4. Type `root@localhost:2222` and press Enter.
   - If it asks for the platform, choose Linux.
   - If it asks whether you trust the host, click Continue.
   - The first connection installs Positron's server in the container and can take several minutes.
5. Open the project folder. The window shows `SSH: localhost:2222`, while the Explorer says no folder is open. Click **Open Folder**, delete the path shown, type `/workspace`, and press Enter. The Explorer then shows the project files.
6. Choose the Python interpreter every time. Click the interpreter button at the top right, choose **New Console Session**, and pick **Python 3.x (uv: penguin-species-classification)** with the path `/opt/venv/bin/python`. If it is not listed, run **Python: Select Interpreter**, choose **Enter interpreter path**, and type `/opt/venv/bin/python`. Do not choose `/workspace/.venv`. In the Console, `import sys; print(sys.executable)` should print `/opt/venv/bin/python`.
7. The Terminal, Console, and Render button run inside the container.
8. When you finish, close the Positron window, then stop the container on your computer:

```bash
docker compose down
```

If it does not connect:

- If `~/.ssh/id_ed25519.pub` is a folder, run `docker compose down`, delete that folder, make the key as described in the README, and start again.
- If `~/.gitconfig` is a folder, or Git in the container does not know your name, run `docker compose down`, delete that folder, set your name and email, and start again.
- If it asks for a password, the container cannot see your key. Check that the key is named `id_ed25519.pub`, or change the key path in `docker-compose.yml`.
- If it says the remote host identification has changed, run `ssh-keygen -R "[localhost]:2222"` and connect again.
- If GitHub asks you to sign in when you push from the container, sign in once in the browser window Positron opens.
- On a Linux laptop, ssh may refuse the key. Remove the `#` from the `StrictModes no` line in the `Dockerfile` and push, so the workflow publishes a fixed image.
- If port 2222 is already in use, change the first `2222` in `docker-compose.yml` to another number, such as `2223`, and connect to that port.

To render the report without opening Positron:

```bash
docker compose run --rm analysis-env uv run quarto render analysis/penguin-species-classification.qmd
```

## Adding or updating a package

On a new branch, add the package with `uv add`, which pins the version in `pyproject.toml` and updates `uv.lock`. Commit `pyproject.toml` and `uv.lock` together and push them. GitHub Actions publishes a new image and commits the new `sha-` tag to `docker-compose.yml` on that branch. When the workflow finishes, run:

```bash
git pull
docker compose pull
```

Then open a pull request to merge the branch into `main`.

## Publishing the report

The report on GitHub Pages is the copy at `docs/index.html`. After you render the report again, copy it, commit, and push:

```bash
cp analysis/penguin-species-classification.html docs/index.html
```

## Pull request process

- A person opens every issue. A person reviews and merges every pull request.
- Create a branch from an up-to-date `main`. Do not commit directly to `main`.
- Give the branch a name that describes the work.
- Open a pull request into `main`, describe what changed and why, and link the issue, for example `Closes #12`.
- At least one other team member reviews and approves the pull request before it is merged.

## Code of Conduct

This project is released with a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to abide by its terms.

## AI Policy

You are responsible for any code you submit to this repository, whether you wrote it or generated it with AI. You must understand and be able to explain the code you submit and the related code around it. Your explanation must be your own, though you may use AI to translate or copy-edit it.

An AI agent may not open, review, or merge a pull request. A person checks any generated code and opens the pull request.

This document was adapted from the [dplyr contributing guidelines](https://github.com/tidyverse/dplyr/blob/main/.github/CONTRIBUTING.md) and [earthaccess's AI policy](https://earthaccess.readthedocs.io/en/latest/contributor/ai-policy/).
