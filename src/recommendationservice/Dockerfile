# -------------------------
# Builder stage

FROM --platform=$BUILDPLATFORM python:3.12-slim AS builder
WORKDIR /app

# Install build dependencies for Python native extensions
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    python3-dev \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies into a separate prefix
COPY requirements.txt .
RUN pip install --upgrade pip && \
    pip install --prefix=/install -r requirements.txt


# -------------------------
# Final runtime image
# -------------------------
FROM python:3.12-slim

ENV PYTHONUNBUFFERED=1
ENV PORT=8080

WORKDIR /recommendationservice

# Copy Python installed packages from builder
COPY --from=builder /install /usr/local

# Copy application code
COPY . .

EXPOSE 8080
ENTRYPOINT ["python", "recommendation_server.py"]
