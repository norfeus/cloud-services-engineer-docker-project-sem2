# Stage 1: Build the Go application
FROM golang:1.23 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /go/bin/app ./cmd/api

# Stage 2: Add healthcheck tools
FROM busybox:uclibc AS busybox

# Stage 3: Minimal runtime image
FROM gcr.io/distroless/static-debian12:nonroot

WORKDIR /app
COPY --from=builder --chown=65532:65532 /go/bin/app .
COPY --from=busybox /bin/wget /usr/bin/wget

USER 65532:65532
EXPOSE 8081

ENTRYPOINT ["/app/app"]