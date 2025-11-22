
# =========================
# Build Stage
# =========================
FROM --platform=$BUILDPLATFORM golang:1.23.4-alpine AS builder

ARG TARGETOS
ARG TARGETARCH
WORKDIR /src

# Download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Build
ARG SKAFFOLD_GO_GCFLAGS
RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} CGO_ENABLED=0 \
    go build -gcflags="${SKAFFOLD_GO_GCFLAGS}" -o /app/server .

# =========================
# Final Stage (Scratch)
# =========================
FROM scratch

WORKDIR /app

COPY --from=builder /app/server /app/server
COPY ./templates ./templates
COPY ./static ./static

ENV GOTRACEBACK=single
EXPOSE 8080

ENTRYPOINT ["/app/server"]
