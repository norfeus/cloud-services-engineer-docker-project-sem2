ARG NODE_VERSION=16.18.0
ARG VUE_APP_API_URL=/momo-store/

FROM node:${NODE_VERSION}-alpine AS builder
# Stage 1: Build the Vue application

WORKDIR /usr/src/app

# Копируем остальной код и собираем
COPY . .
RUN npm install
RUN npm run build

# Stage 2: Serve with nginx
FROM nginx:1.25-alpine

# Убираем дефолтную конфигурацию
RUN rm -rf /etc/nginx/conf.d

# Копируем наш dist
RUN rm -rf /usr/share/nginx/html
COPY --from=builder /usr/src/app/dist /usr/share/nginx/html
# Копируем кастомную конфигурацию Nginx
#COPY --from=builder /usr/src/app/nginx.conf /etc/nginx/nginx.conf
#COPY --from=builder /usr/src/app/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /usr/src/app/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8081

CMD ["nginx", "-g", "daemon off;"]
