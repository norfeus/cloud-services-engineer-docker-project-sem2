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
COPY --from=builder /go/bin/app .
COPY --from=builder /bin/wget /usr/bin/wget
USER 65532:65532
EXPOSE 8081
CMD ["/app/app"]