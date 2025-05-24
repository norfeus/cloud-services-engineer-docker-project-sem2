# Stage 1: Build the Go application
FROM golang:1.23 as builder

WORKDIR /app

# Копируем только go.mod/go.sum для кэширования зависимостей
COPY go.mod go.sum ./
RUN go mod download

# Копируем исходный код и компилируем
COPY . .
RUN CGO_ENABLED=0 go build -o momo-store ./cmd/api

# Stage 2: Minimal runtime image
FROM gcr.io/distroless/static-debian12

RUN addgroup -g 1001 -S mygroup && \
    adduser -u 1001 -S myuser -G mygroup

# Создаём рабочую директорию
WORKDIR /app

# Копируем бинарник из предыдущего этапа
COPY --from=builder /app/momo-store .

# Создаём непривилегированного пользователя
#RUN adduser --disabled-password --gecos '' myuser

USER myuser
EXPOSE 8080

# Запускаем приложение
CMD ["/app"]
