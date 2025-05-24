# Stage 1: Build the Go application
FROM golang:1.23 AS builder

WORKDIR /app

# Копируем только go.mod/go.sum для кэширования зависимостей
COPY go.mod go.sum ./
RUN go mod download

# Копируем исходный код и компилируем
COPY . .
RUN CGO_ENABLED=0 go build -o momo-store ./cmd/api

# Stage 2: Minimal runtime image
FROM alpine:3.19

# Создаём рабочую директорию
WORKDIR /app

# Копируем бинарник из предыдущего этапа
COPY --from=builder /app/momo-store .

# Создаём непривилегированного пользователя
#RUN adduser --disabled-password --gecos '' myuser

RUN adduser -D -g '' myuser && chown -R myuser /app
USER myuser

EXPOSE 8080

# Запускаем приложение
CMD ["/app/momo-store"]
