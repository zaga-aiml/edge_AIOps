# ✅ Use bookworm-based image (Podman CI safe)
FROM python:3.11-slim-bookworm

# Set the working directory
WORKDIR /app

# Install system build dependencies (single layer)
RUN apt-get update && \
    apt-get install -y \
        gcc \
        python3-dev \
        build-essential \
        wget \
    && rm -rf /var/lib/apt/lists/*

    

# Copy requirement files and install dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy the rest of your application
COPY . .

# Run FastAPI app
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8001"]
