# quarto-jupyter-cache-issue-repro

A simplified issue reproduction example.

## The issue

In a website project with Python code cells where I've enabled caching (using Jupyter Cache)
I want to be able to control where Jupyter Cache writes its files, but their documented method
(setting the `JUPYTERCACHE` env var) doesn't seem to work.

## Setup

Install quarto (v1.6.40) and Python (probably any recent v3 is fine, I'm using v3.11).
Set up a virtual environment and install the project dependencies.

```bash
# Install Python deps (Linux version)

# EITHER: vanilla Python...
python3.11 -m venv .venv
source .venv/bin/activate
pip install .

# OR: if you have uv installed...
uv sync
```

## Repro steps

```bash
# Activate venv
source .venv/bin/activate

# Set JUPYTERCACHE var
export JUPYTERCACHE=./my-jupyter-cache

# Render the site
quarto render --to html
```

**Expected**: I was hoping for a folder named `my-jupyter-cache` at project root containing the cache files
for all notebooks.

**Actual**: Two folders named `.jupyter_cache` are written, one at project root and one in the examples folder.
This suggests the `JUPYTERCACHE` setting is not being used.

I have also tried setting `JUPYTERCACHE` in the `_environment` file, also to no effect. I also tried variations
of the path, without the leading dot and absolute paths.

Note: a re-run of `quarto render --to html` will show that notebooks are read from cache,
demonstrating that caching in general does function.
