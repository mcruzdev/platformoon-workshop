# Platformoon Docs
Welcome to the documentation for Platformoon Docs! This README will guide you on how to set up and run this documentation locally.

## Prerequisites

Before you start, make sure you have the following prerequisites installed on your system:

- [Python](https://www.python.org/downloads/) (3.6 or higher)
- [pip](https://pip.pypa.io/en/stable/installing/)
- [MkDocs](https://www.mkdocs.org/#installation)
- [MkDocs-Material](https://squidfunk.github.io/mkdocs-material/getting-started/)

## Getting Started

1. Clone the repository to your local machine:

   ```bash
   git clone <repository_url>
   ```
2. Change your working directory to the project's root folder:
    ```bash
    cd <repository_directory>
    ```
3. Install packages:
    ```bash
    pip install mkdocs
    pip install mkdocs-material
    ```
4. Build the documentation:
    ```bash
    mkdocs build
    ```
5. Serve the documentation locally:
    ```bash
    mkdocs serve
    ```
6. Open your web browser and navigate to http://localhost:8000 to view the documentation.

### Contibution Guide

If you'd like to contribute to this documentation, follow these steps:

1. Fork this repository on GitHub.

2. Create a new branch for your changes:
    ```bash
    git checkout -b add-new-docs
    ```
3. Make your changes to the documentation.

4. Test your changes locally by following the "Getting Started" instructions.

5. Commit your changes and push them to your forked repository:
    ```bash
    git add .
    git commit -m "Add new documentation"
    git push origin add-new-docs
   ```
6. Create a Pull Request (PR) on the original repository, explaining your changes and why they should be merged.
