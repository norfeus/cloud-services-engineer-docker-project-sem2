# Stage 1: Build the Go application
FROM golang:1.23 AS builder

WORKDIR /app

# Копируем только go.mod/go.sum для кэширования зависимостей
COPY go.mod go.sum ./
RUN go mod download

# Копируем исходный код и компилируем
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -C cmd/api -a -installsuffix cgo -o momo-store 

# Stage 2: Minimal runtime image
FROM gcr.io/distroless/static-debian12

# Создаём рабочую директорию
WORKDIR /app

# Копируем бинарник из предыдущего этапа
COPY --from=builder /app/momo-store .

# Используем UID 65532 (nobody)
USER 65532:65532

EXPOSE 8081

# Запускаем приложение
CMD ["/app/momo-store"]