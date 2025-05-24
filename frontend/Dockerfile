
ARG NODE_VERSION=18
ARG VUE_APP_API_URL

# Stage 1: Build the Vue application
FROM node:${NODE_VERSION}-alpine AS builder
WORKDIR /usr/src/app
COPY . .
ENV NODE_OPTIONS=--openssl-legacy-provider
RUN npm ci && VUE_APP_API_URL=$VUE_APP_API_URL npm run build 

# Stage 2: Serve with nginx
FROM nginx:1.25-alpine
RUN  touch /var/run/nginx.pid && \
     chown -R nginx:nginx /var/cache/nginx /var/run/nginx.pid
COPY --from=builder --chown=nginx:nginx /usr/src/app/dist /usr/share/nginx/html
COPY --from=builder --chown=nginx:nginx /usr/src/app/nginx.conf /etc/nginx/conf.d/default.conf
USER nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
