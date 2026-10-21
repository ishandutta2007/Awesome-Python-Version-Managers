# Awesome-Python-Version-Managers

## Top Python Version Managers Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Interpreter Isolation, Unified Toolchains & Reproducible Environments*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Python Version Managers**. These tools install, switch, and pin Python interpreters across projects — so you can run 3.11 on one repo and 3.13 on another without PATH conflicts, while keeping tooling like Node or Ruby managed by the same command.

**Examples** include pyenv, uv, mise, asdf, Conda, Poetry, Pipenv, Hatch, and virtualenv (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, reproducible environment pinning, and transparent interpreter management — ideal for Python developers tired of "which python am I actually running?" debugging, polyglot teams managing multiple runtimes, and CI pipelines needing deterministic versions.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **GitHub Actions (actions/setup-python)** — Managed CI runner action for installing and pinning Python versions in GitHub Actions workflows. Reads `.python-version` files and supports version matrix testing across multiple interpreters. Free for public repositories with usage limits on private repos.

- **Google Cloud Build** — Managed CI/CD service with support for Python version selection via build steps and container images. Integrates with Google Cloud ecosystem.

- **AWS CodeBuild** — Managed build service with Python runtime images for CI/CD pipelines. Pay-per-minute pricing for build compute.

## Open-Source GitHub Projects

- **[pyenv](https://github.com/pyenv/pyenv)**  
  **The long-standing standard for Python version management** — MIT licensed, **42,000+ GitHub stars** . **Simple, unobtrusive Python version management** that compiles interpreters from source for optimal platform-specific builds . **Per-project pinning via `.python-version`** — commit the file and collaborators get the same Python automatically [citation:1]. **Shims** intercept `python`, `pip`, and other commands to route to the active version . **The most widely documented and battle-tested option** — countless Stack Overflow answers and CI integrations assume pyenv . **Trade-offs**: **compiles from source by default**, which is slow (several minutes per version) and requires system build dependencies (`zlib1g-dev`, `python3-dev`, etc.) [citation:8]. **Best for teams wanting the most established, predictable Python-only version manager** with broad ecosystem compatibility.

- **[uv](https://github.com/astral-sh/uv)**  
  **The Rust-powered all-in-one Python toolchain** — MIT/Apache dual licensed, from the creators of Ruff . **A single tool to replace `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, and `virtualenv`** [citation:10]. **10-100x faster than pip** for dependency resolution and installation [citation:10]. **`uv python install 3.12 3.13 3.14`** installs multiple interpreters in **under a second** (972ms for three versions in Astral's example) [citation:10]. **Cross-platform lockfile** (`uv.lock`) supports workspaces and reproducible CI [citation:14]. **`uv run`** executes commands in the project venv without manual activation — the "bread and meat" of the workflow [citation:14]. **Precompiled Python builds use PGO and BOLT optimizations** that pyenv does not enable by default [citation:14]. **`mise sync python --uv`** enables two-way sharing of interpreter installs [citation:7]. **Best for Python-only teams wanting the fastest, most integrated toolchain** — but note it **only supports PEP 517 builds, not legacy `setup.py`** [citation:8].

- **[mise](https://github.com/jdx/mise)**  
  **The polyglot version manager that supersedes asdf** — MIT licensed, Rust-based . **Manages Python, Node, Go, Ruby, Java, Terraform, and dozens more** with one tool and one config [citation:1]. **`mise use python@3.13 node@22`** pins both runtimes in a project directory [citation:11]. **`.tool-versions` or `.mise.toml`** per-project configs auto-activate on `cd` [citation:1][citation:11]. **Precompiled Python installs by default**, falling back to `python-build` (pyenv's compiler) only when needed [citation:2]. **`mise sync python --pyenv`** imports existing pyenv installs without reinstalling [citation:7]. **Also manages environment variables and tasks** (`mise run test`), absorbing `.env` and `Makefile` [citation:11]. **Trade-offs**: **Python compilation can fail if system dev packages are missing** (`Python.h: No such file or directory`) because mise does not auto-install them [citation:8]. **Best for polyglot teams** managing multiple language runtimes — pure-Python setups have less overhead with uv or pyenv [citation:1].

- **[asdf](https://github.com/asdf-vm/asdf)**  
  **The original multi-language version manager** — MIT licensed, **0.20.2 stable** [citation:4]. **Plugin system** for Python, Node, Ruby, Go, Java, Rust, and dozens more [citation:1]. **`.tool-versions`** holds multiple language versions in one file — cleaner than one file per language for polyglot projects [citation:1]. **`asdf local python 3.13.1`** writes the file; **`asdf global`** sets the default [citation:1]. **mise is the Rust rewrite of asdf** with faster startup and slightly different config format — most users migrating from asdf find mise reads `.tool-versions` seamlessly [citation:1][citation:11]. **Best for teams already standardized on asdf** or needing its mature plugin ecosystem.

- **[Conda](https://github.com/conda/conda)**  
  **The package and environment manager that also manages Python versions** — BSD-3-Clause licensed . **Each conda environment contains its own Python**, so version switching is a side effect of environment activation [citation:1]. **Strongest for data science and scientific computing** — conda-forge provides precompiled binaries for NumPy, SciPy, PyTorch, and other C-extension-heavy packages that often fail to build from source . **Heavier than uv or pyenv for pure-Python projects** [citation:1]. **Best for scientific computing teams** where binary dependency management matters more than minimal overhead.

- **[Poetry](https://github.com/python-poetry/poetry)**  
  **Dependency management and packaging with Python version constraints** — MIT licensed . **`pyproject.toml` + `poetry.lock`** for reproducible installs . **Does not install Python interpreters itself** — relies on pyenv, uv, or system Python for the actual runtime . **Plugin ecosystem and established workflow habits** are retention reasons against uv migration [citation:11]. **Best for teams wanting mature dependency locking** with a separate interpreter manager.

- **[Pipenv](https://github.com/pypa/pipenv)**  
  **The officially recommended packaging tool from PyPA** — MIT licensed . **Combines pip and virtualenv** with `Pipfile` and `Pipfile.lock` . **Does not manage Python interpreters** — requires an existing Python installation. **Less actively developed** than Poetry or uv, but still used in legacy projects.

- **[Hatch](https://github.com/pypa/hatch)**  
  **Modern project management and packaging tool** — MIT licensed . **Manages environments, dependencies, and builds** with `pyproject.toml` . **`hatch env`** creates isolated environments per Python version . **Works well with uv's lockfiles** according to some users, though one HN commenter noted dropping Hatch because it "does not work well with uv's lockfiles" [citation:14]. **Best for teams wanting a batteries-included project manager** with modern defaults.

- **[virtualenv](https://github.com/pypa/virtualenv)**  
  **The original Python environment isolation tool** — MIT licensed . **Creates isolated Python environments** without touching system Python . **`python -m venv` is the standard library equivalent** since Python 3.3 . **Does not manage Python versions** — requires an existing interpreter. **uv venv is 3-5x faster** and includes pip and setuptools by default [citation:8]. **Best for minimal environments** where you already have the interpreter you need.

### Additional Strong Open-Source Options

- **py launcher (Windows)** — Windows-only launcher that respects `.python-version` files and selects among installed Pythons [citation:1].
- **rye** — Astral's experimental Python project manager, now largely superseded by uv.
- **PDM** — Modern Python package manager with PEP 582 support and lockfiles.
- **pixi** — Fast package management built on conda-forge with a Rust-based solver.
- **pipx** — Install and run Python CLI tools in isolated environments. Often paired with pyenv [citation:8].

**Frameworks for building custom systems**: Combine **uv** for the fastest Python-only workflow with interpreter installation, dependency locking, and project management in one tool, **mise** for polyglot teams managing Python alongside Node, Go, and other runtimes with project-local pinning, **pyenv** for the most established Python-specific version manager with broad ecosystem compatibility, **Conda** for scientific computing where binary dependencies matter, and **Poetry** or **Hatch** for mature dependency locking if you already have an interpreter manager — to build a Python toolchain tailored to your team's language mix, dependency complexity, and CI requirements.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Python version managers differ significantly in scope, speed, and ecosystem compatibility. **pyenv compiles from source**, which is slow but produces platform-optimized builds; **uv installs precompiled builds** with PGO/BOLT optimizations that are typically faster to install and often faster to run [citation:14]. **uv only supports PEP 517 builds** — legacy projects with only `setup.py` must fall back to pip [citation:8]. **mise can fail to compile Python if system dev packages are missing** (`Python.h` errors) because it does not auto-install them [citation:8]. **Do not let two tools manage the same Python version** — pick mise or uv for interpreters, and let the other handle packages, to avoid PATH conflicts [citation:11]. **macOS has three Pythons fighting for PATH** (`/usr/bin/python3`, Homebrew Python, and your manager's Python) — use `which python` and `python -c "import sys; print(sys.executable)"` to diagnose [citation:1].
- Self-hosted open-source solutions require proper `.python-version` or `.tool-versions` files committed to repositories for reproducibility, and CI configuration that respects those files.

---

**Made for Python developers, data scientists, polyglot engineers, and DevOps teams managing interpreter environments.**
Let's make Python version management more open, reproducible, and toolchain-coherent.
