# Stage 1: Build the Go application
FROM golang:1.23 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /go/bin/app ./cmd/api
#RUN chmod a+x /usr/bin/curl
#ADD https://github.com/jungsoft/docker-healthcheck/releases/download/0.1.0/healthcheck_alpine_amd64 /usr/local/bin/healthcheck
#RUN chmod +x /usr/local/bin/healthcheck
#ADD --chown=65532:65532 https://github.com/jungsoft/docker-healthcheck/releases/download/0.1.0/healthcheck_alpine_amd64 /usr/local/bin/healthcheck
#RUN chmod a+x /usr/local/bin/healthcheck

# Stage 2: Add healthcheck tools
#FROM busybox:1.35.0-uclibc as busybox

FROM debian:12-slim AS healthchecker
RUN apt update && \
    apt install -y --no-install-recommends git curl gnupg && \
    rm -rf /var/lib/apt/lists/*

#FROM alpine:3.19 AS healthchecker
#RUN apk add --no-cache wget

# Stage 2: Minimal runtime image
FROM gcr.io/distroless/static-debian12

WORKDIR /app
COPY --from=builder --chown=65532:65532 /go/bin/app .
COPY --from=healthchecker /usr/bin/curl /usr/bin/curl

#COPY --from=busybox:1.35.0-uclibc /bin/sh /bin/sh

USER 65532:65532
EXPOSE 8081

CMD ["/app/app"]