# Stage 1: Build the Go application
FROM golang:1.23 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /go/bin/app ./cmd/api

# Stage 2: Minimal runtime image
FROM gcr.io/distroless/static-debian12
WORKDIR /app
COPY --from=builder --chown=65532:65532 /go/bin/app .
COPY --from=builder --chown=65532:65532 /usr/bin/curl /usr/bin/curl
USER 65532:65532
EXPOSE 8081

HEALTHCHECK --interval=10s \
            --timeout=5s \
            CMD ["/usr/bin/curl", "-f", "http://localhost:8081/health"]

CMD ["/app/app"]