FROM python:3.11-slim-bookworm

WORKDIR /app

RUN apt-get update && apt-get install -y \
        curl \
        gcc \
        python3-dev \
        build-essential \
        wget \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /app /tmp/huggingface \
    && chmod -R 777 /app /tmp/huggingface

ENV HF_HOME=/tmp/huggingface
ENV TRANSFORMERS_CACHE=/tmp/huggingface/transformers
ENV PYTHONPATH=/app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8002"]
