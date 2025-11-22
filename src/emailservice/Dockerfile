# Build stage
FROM --platform=$BUILDPLATFORM python:3.12-slim AS builder
WORKDIR /app

# Install system dependencies required for pip builds
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python deps
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Runtime stage
FROM python:3.12-slim

# Enable unbuffered logs
ENV PYTHONUNBUFFERED=1
ENV ENABLE_PROFILER=1

WORKDIR /email_server

# Copy installed dependencies
COPY --from=builder /usr/local/lib/python3.12 /usr/local/lib/python3.12

# Copy application code
COPY . .

EXPOSE 8080
ENTRYPOINT ["python", "email_server.py"]

