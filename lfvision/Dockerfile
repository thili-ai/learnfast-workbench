# Learn Fast — Computer Vision workbench (drills: lfcnn, lfgan, lfquantization, lfvit)
#
# An EMPTY environment: every library the vision drills need, and none of the course notebooks.
# You write your own notebooks in work/ — that directory is what you submit for the drill.
FROM quay.io/jupyter/scipy-notebook:python-3.12

# NOTE: the user is renamed jovyan -> learnfast at RUN time, in docker-compose.yml, not here.
# Renaming it in the image looks tidier but breaks the base image: upstream's start.sh runs
# `JOVYAN_UID="$(id -u jovyan)"` under `set -e`, so once that account no longer exists the
# container dies on startup with exit 1 and no error message. The runtime path (NB_USER +
# CHOWN_HOME, started as root) is the one upstream supports and maintains.

# CPU-only torch, installed from PyTorch's cpu index. The default PyPI wheels drag in the CUDA
# runtime — gigabytes of download for a course that is deliberately GPU-free.
RUN pip install --no-cache-dir --index-url https://download.pytorch.org/whl/cpu \
      torch torchvision

# numpy / pandas / matplotlib / pillow / requests already ship in the scipy-notebook base.
# Kept at /tmp rather than removed: the image runs as a non-root user, and a root-owned COPY can't be
# deleted in the same layer without a chown dance that buys nothing.
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt
