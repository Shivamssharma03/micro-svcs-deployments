# builder stage: install build deps & Python libs
FROM --platform=$BUILDPLATFORM python:3.12-slim AS builder
WORKDIR /app

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    build-essential \
    wget \
    ca-certificates \
    gcc \
    libffi-dev \
    libssl-dev \
    python3-dev \
 && rm -rf /var/lib/apt/lists/*

# Copy requirements and install into an isolated prefix
COPY requirements.txt .
RUN pip install --upgrade pip \
 && pip install --prefix=/install -r requirements.txt \
 && rm -rf /root/.cache/pip

# final runtime image (smaller)
FROM python:3.12-slim
WORKDIR /loadgen

# runtime deps (only required runtime packages)
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    ca-certificates \
 && rm -rf /var/lib/apt/lists/*

# copy installed Python packages from builder
COPY --from=builder /install /usr/local

# copy only what's needed
COPY locustfile.py .

# environment (enable gevent support in any debugger)
ENV GEVENT_SUPPORT=True

# default entrypoint — uses env vars FRONTEND_ADDR, USERS, RATE
ENTRYPOINT ["sh", "-c", "exec locust --host=\"http://${FRONTEND_ADDR}\" --headless -u \"${USERS:-10}\" -r \"${RATE:-1}\" 2>&1"]
