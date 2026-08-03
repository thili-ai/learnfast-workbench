# Learn Fast — ML for Security workbench
#
# An EMPTY environment: the libraries this track needs, and none of the course notebooks.
# You write your own notebooks in work/ — that folder is what you submit for the drill.
FROM quay.io/jupyter/scipy-notebook:python-3.12

# NOTE: the user is renamed jovyan -> learnfast at RUN time (see docker-compose.yml), never here:
# upstream's start.sh calls `id -u jovyan` under `set -e`, so renaming in the image makes the
# container exit 1 on startup with no error message.

# CPU-only torch: the default PyPI wheels drag in the CUDA runtime, which is gigabytes of
# download for a course that is deliberately GPU-free.
RUN pip install --no-cache-dir --index-url https://download.pytorch.org/whl/cpu \
      torch

# numpy / pandas / matplotlib / scipy / scikit-learn / pillow already ship in the base image.
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt
