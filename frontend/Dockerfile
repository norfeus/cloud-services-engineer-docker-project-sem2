# Stage 1: Build the Vue application
FROM node:18-alpine AS builder

RUN apk update && \
    apk upgrade && \
    apk add --no-cache python3 make g++

WORKDIR /app

ENV NODE_ENV=development
ENV VUE_APP_API_URL=/api
ENV NODE_OPTIONS=--openssl-legacy-provider

# Копируем только package.json и package-lock.json для кэширования зависимостей
COPY package.json package-lock.json ./
RUN npm ci --no-audit --no-fund

#COPY package.json package-lock.json ./
#RUN npm ci --only=production



# Копируем исходные файлы и собираем приложение
COPY . .
RUN npm run build

# Stage 2: Serve the application with nginx
FROM nginx:alpine

# Убираем дефолтные настройки nginx
RUN rm -rf /etc/nginx/conf.d

# Копируем кастомную конфигурацию (если есть), или используем стандартную
COPY --from=builder /app/dist /usr/share/nginx/html

# Добавляем собственную конфигурацию Nginx (настраиваем корневую директорию и т.д.)
COPY nginx.conf /etc/nginx/nginx.conf

# Открываем порт 80
EXPOSE 80

# Запускаем Nginx в foreground режиме
CMD ["nginx", "-g", "daemon off;"]