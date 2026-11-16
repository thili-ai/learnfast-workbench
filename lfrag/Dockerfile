# Learn Fast — RAG workbench
#
# An EMPTY environment: the libraries this track needs, and none of the course notebooks.
# You write your own notebooks in work/ — that folder is what you submit for the drill.
FROM quay.io/jupyter/minimal-notebook:python-3.12

# NOTE: the user is renamed jovyan -> learnfast at RUN time (see docker-compose.yml), never here:
# upstream's start.sh calls `id -u jovyan` under `set -e`, so renaming in the image makes the
# container exit 1 on startup with no error message.

# Tesseract is the OCR program the scan lessons run. apt needs root; switch back after.
USER root
RUN apt-get update \
 && apt-get install -y --no-install-recommends tesseract-ocr \
 && rm -rf /var/lib/apt/lists/*
USER ${NB_UID}

# CPU-only torch (sentence-transformers needs it): the default wheels drag in the CUDA runtime,
# gigabytes of download for a course that is deliberately GPU-free.
RUN pip install --no-cache-dir --index-url https://download.pytorch.org/whl/cpu torch

# The reference library, cloned at a fixed tag. We clone (not only pip install) because the lessons
# also read its sample documents, questions and saved model replies, which are not in the pip package.
# THILIRAG_DIR (set below) tells the notebooks where that folder is.
ARG THILIRAG_TAG=v0.1.0
USER root
RUN git clone --depth 1 --branch ${THILIRAG_TAG} https://github.com/thili-ai/thilirag.git /opt/thilirag \
 && chown -R ${NB_UID}:${NB_GID} /opt/thilirag
USER ${NB_UID}
ENV THILIRAG_DIR=/opt/thilirag

COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# The embedding model (all-MiniLM-L6-v2, about 90 MB) is NOT baked in, to keep the image small.
# sentence-transformers downloads it the first time a lesson loads it, then caches it in the container.
