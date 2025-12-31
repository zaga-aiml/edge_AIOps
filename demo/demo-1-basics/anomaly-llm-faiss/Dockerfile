# ✅ Podman + GitHub Actions safe base image
FROM python:3.11-slim-bookworm

WORKDIR /app

# -----------------------------
# System deps (MINIMAL)
# -----------------------------
RUN apt-get update && apt-get install -y \
        gcc \
        python3-dev \
        build-essential \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------
# Runtime dirs (OpenShift-safe)
# -----------------------------
RUN mkdir -p /app /tmp/huggingface /tmp/pip-cache \
    && chmod -R 777 /app /tmp/huggingface /tmp/pip-cache

# -----------------------------
# Environment hard fixes
# -----------------------------
ENV PYTHONPATH=/app
ENV PYTHONUNBUFFERED=1

# HuggingFace cache
ENV HF_HOME=/tmp/huggingface
ENV TRANSFORMERS_CACHE=/tmp/huggingface/transformers

# 🚫 Podman disk explosion fixes
ENV PIP_NO_CACHE_DIR=1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV PIP_CACHE_DIR=/tmp/pip-cache
ENV PIP_PREFER_BINARY=1

# 🚫 HARD block GPU / CUDA wheels
ENV CUDA_VISIBLE_DEVICES=""
ENV TORCH_CUDA_ARCH_LIST=""
ENV TRANSFORMERS_NO_TF=1
ENV TRANSFORMERS_NO_FLAX=1

# -----------------------------
# Python deps (single layer)
# -----------------------------
COPY requirements.txt .
RUN pip install --no-cache-dir --prefer-binary -r requirements.txt \
    && rm -rf /tmp/pip-cache

# -----------------------------
# App source
# -----------------------------
COPY . .

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8002"]
