# Learn Fast — Travel desk workbench
#
# An EMPTY environment: the libraries this drill uses, and none of the course notebooks.
# You write your own notebooks in work/ — that folder is what you submit for the drill.
FROM quay.io/jupyter/minimal-notebook:python-3.12

# NOTE: the user is renamed jovyan -> learnfast at RUN time (see docker-compose.yml), never here:
# upstream's start.sh calls `id -u jovyan` under `set -e`, so renaming in the image makes the
# container exit 1 on startup with no error message.

# minimal-notebook ships JupyterLab and nothing else, so every library this drill uses is listed
# explicitly. There is no torch here: training happens on Google Colab, and the converted model runs
# with llama.cpp on your own machine (see docker-compose.yml).
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt
