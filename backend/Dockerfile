# Stage 1: Build the Go application
FROM golang:1.23 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /go/bin/app ./cmd/api
#RUN chmod a+x /usr/bin/curl
ADD https://github.com/jungsoft/docker-healthcheck/releases/download/0.1.0/healthcheck_alpine_amd64 /usr/local/bin/healthcheck
RUN chmod +x /usr/local/bin/healthcheck


# Stage 2: Minimal runtime image
FROM gcr.io/distroless/static-debian12
WORKDIR /app
COPY --from=builder --chown=65532:65532 /go/bin/app .
COPY --from=builder --chown=65532:65532 /usr/local/bin/healthcheck /usr/local/bin/healthcheck



USER 65532:65532
EXPOSE 8081

HEALTHCHECK --interval=10s \
            --timeout=5s \
            CMD ["/usr/local/bin/healthcheck", "-f", "http://localhost:8081/health"]

CMD ["/app/app"]