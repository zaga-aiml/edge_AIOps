# ✅ Podman + OpenShift safe base image
FROM python:3.11-slim-bookworm

# Install build dependencies (single layer)
RUN apt-get update && apt-get install -y \
        curl \
        gcc \
        python3-dev \
        build-essential \
    && rm -rf /var/lib/apt/lists/*

# OpenShift-compatible writable directories
RUN mkdir -p /app /tmp/huggingface /mnt/hf_cache \
    && chmod -R 777 /app /tmp/huggingface /mnt/hf_cache

# Set the working directory
WORKDIR /app

# Hugging Face cache locations
ENV HF_HOME=/tmp/huggingface
ENV TRANSFORMERS_CACHE=/tmp/huggingface/transformers

# Copy requirements first for layer caching
COPY requirements.txt .

# Install Python dependencies
RUN pip install -r requirements.txt

# Copy application code
COPY . .

# Ensure imports work
ENV PYTHONPATH=/app

# Run FastAPI
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8002", "--log-level", "debug", "--access-log"]
