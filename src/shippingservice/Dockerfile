# ---------- BUILD STAGE ----------
FROM golang:1.23.4-alpine AS builder

WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download

COPY . .

# build for linux (the docker image runtime)
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o shippingservice .

# ---------- RUNTIME STAGE ----------
FROM scratch

WORKDIR /app

COPY --from=builder /src/shippingservice .

ENV APP_PORT=50051
ENV GOTRACEBACK=single

EXPOSE 50051

ENTRYPOINT ["./shippingservice"]
