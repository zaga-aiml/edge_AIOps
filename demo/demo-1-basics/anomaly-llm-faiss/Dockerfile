FROM python:3.11-slim-bookworm

# -------------------------------
# Working directory
# -------------------------------
WORKDIR /app

# -------------------------------
# System dependencies (single layer)
# -------------------------------
RUN apt-get update && apt-get install -y \
        curl \
        gcc \
        python3-dev \
        build-essential \
        wget \
    && rm -rf /var/lib/apt/lists/*

# -------------------------------
# Directories required for OpenShift / HuggingFace
# (OpenShift runs with random UID)
# -------------------------------
RUN mkdir -p /app \
             /tmp/huggingface \
             /tmp/huggingface/transformers \
    && chmod -R 777 /app /tmp/huggingface

# -------------------------------
# Environment hardening (CRITICAL)
# -------------------------------
ENV PYTHONPATH=/app
ENV PYTHONUNBUFFERED=1

# HuggingFace caches
ENV HF_HOME=/tmp/huggingface
ENV TRANSFORMERS_CACHE=/tmp/huggingface/transformers

# 🚫 Disable pip cache (fixes overlayfs failures)
ENV PIP_NO_CACHE_DIR=1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1

# 🚫 Prevent CUDA / NVIDIA probing
ENV CUDA_VISIBLE_DEVICES=""
ENV TORCH_CUDA_ARCH_LIST=""

# -------------------------------
# Python dependencies
# -------------------------------
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# -------------------------------
# Application source
# -------------------------------
COPY . .

# -------------------------------
# Run FastAPI
# -------------------------------
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8002"]
