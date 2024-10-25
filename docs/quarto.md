
# Quarto

[Quarto](https://quarto.org/) is a publishing tool that can generate different kinds of projects, including books, websites, and scholarly manuscripts.


## Installation

To use Quarto, you need to install it onto your local machine. You can [download](https://quarto.org/docs/get-started/), or [install via homebrew](https://formulae.brew.sh/cask/quarto) (if you like that kind of thing):

```sh
brew install --cask quarto
```

If you use VS Code, you can also consider installing the [Quarto Extension](https://marketplace.visualstudio.com/items?itemName=quarto.quarto).


## Project Initialization

First create a software repository or parent folder to house the project, for example "my-quarto-project".

Within in the repository, initialize a quarto project of a specific type (book, website, manuscript, etc):

```sh
quarto create
```

Follow the menu options, choosing to create a project of type "manuscript".

Consider initializing a project in a repository's "docs" subdirectory, to stay organized. The commands below assume your Quarto project is in a "docs" subdirectory.

## Organizing

See the "_quarto.yml" file, which defines the organizational structure of the content, including a table of contents.

## Editing

Edit files comprising your content, using Quarto Markdown files with the .qmd extension.

## Building

Previewing the site (runs like a local webserver):

```sh
quarto preview docs/
```

Rendering the site (writes local HTML files to the "docs/_build" directory, which is ignored from version control):

```sh
quarto render docs/
```


## Website Publishing with GitHub Actions

We are using this "deploy.yml" workflow configuration file (see below) to deploy the site to GitHub Pages when new commits are pushed to the main branch.

In order for this to work, you first need to configure your GitHub Pages repo settings to publish via GitHub Actions.


```sh
# https://jupyterbook.org/en/stable/publish/gh-pages.html

name: deploy

on:
  push:
    branches:
    - main
  workflow_dispatch:

jobs:
  deploy-book:
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
    - uses: actions/checkout@v3

    #
    # PYTHON STUFF
    #
    #
    #- name: Set up Python 3.11
    #  uses: actions/setup-python@v4
    #  with:
    #    python-version: 3.11
    #
    #- name: Install docs dependencies
    #  run: |
    #    pip install -r docs/requirements.txt

    - name: Set up Quarto
      uses: quarto-dev/quarto-actions/setup@v2
      with:
        tinytex: true

    - name: Build the book
      run: |
        quarto render docs/

    - name: Upload artifact
      uses: actions/upload-pages-artifact@v2
      with:
        path: "docs/_build"

    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v2

```
