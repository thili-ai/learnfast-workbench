# Learn Fast — Agentic Design Patterns workbench
#
# An EMPTY environment: the libraries these drills need, and none of the course notebooks.
# You write your own notebooks in work/ — that folder is what you submit for the drill.
FROM quay.io/jupyter/minimal-notebook:python-3.12

# NOTE: the user is renamed jovyan -> learnfast at RUN time (see docker-compose.yml), never here:
# upstream's start.sh calls `id -u jovyan` under `set -e`, so renaming in the image makes the
# container exit 1 on startup with no error message.

# No torch/CUDA concerns here — these drills call a hosted model (Groq's free tier) through the
# OpenAI-compatible SDK. minimal-notebook ships JupyterLab and nothing else, so every library the
# lessons use is listed explicitly.
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt && \
    # The 'Brotli' package's compiled process() doesn't accept the output_buffer_limit kwarg
    # that httpx2's BrotliDecoder calls it with — a real incompatibility, not a config error.
    # Nothing here actually needs brotli (gzip/deflate cover it); removing it stops httpx2 from
    # ever negotiating 'br' and hitting the broken decode path.
    pip uninstall -y Brotli brotlicffi 2>/dev/null || true
