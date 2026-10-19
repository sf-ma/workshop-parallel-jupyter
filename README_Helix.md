# Setup on bwForCluster Helix

## Notes

The `helix` branch includes adjustments to `main` for users who want to start their own JupyterLab instance on bwForCluster Helix.
This allows you to test the Dask workflow with non-local workers on Helix as well.

A few hints:

- `git` is only available on the login nodes, not on the compute nodes.
- The deep learning (and machine learning) notebooks should be run on compute nodes with a GPU.
- Some deep learning notebooks require the CUDA module (see instructions below).
- `tensorboard` (used at the end of `08_Deep_Learning/02_PyTorch/06_Solution.ipynb`) is not compatible with Python 3.13.

Most of the notebooks can also be used on [bwVisu](https://wiki.bwhpc.de/e/Helix/bwVisu).
However, certain commands required by the Dask notebooks are not available on bwVisu.
The setup instructions for the Python environment still apply.
After starting bwVisu you must load the Python 3.13.1 module via the side menu, accessible through the double-hexagon button, and select the `Workshop` kernel that was installed during setup.

## Login to Helix

https://wiki.bwhpc.de/e/Helix/Login

## Get Code

```bash
git clone https://github.com/hpcraink/workshop-parallel-jupyter.git
cd workshop-parallel-jupyter
git checkout helix
```

## Setup Python Environment (once)

```bash
ml load devel/python/3.13.1
uv python pin 3.13.1
uv sync --all-extras
uv run python -m ipykernel install --user --name workshop --display-name "Workshop"
```

## Setup JupyterLab (once)

```bash
ml load devel/python/3.13.1
uv tool install jupyterlab --with pip
jupyter-labextension disable dask-labextension
```

## Start JupyterLab

<PORT> should be a number greater than 1023.

```bash
ml load devel/python/3.13.1
ml load devel/cuda/13.3
jupyter-lab --no-browser --ip=0.0.0.0 --port=<PORT>
```
