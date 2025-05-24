
ARG NODE_VERSION=18
ARG VUE_APP_API_URL

FROM node:${NODE_VERSION}-alpine AS builder
# Stage 1: Build the Vue application
WORKDIR /usr/src/app
COPY package*.json .
RUN npm ci
COPY . .
#ENV NODE_ENV=production
ENV NODE_OPTIONS=--openssl-legacy-provider


RUN VUE_APP_API_URL=$VUE_APP_API_URL npm run build 

# Stage 2: Serve with nginx
FROM nginx:1.25-alpine

RUN rm -rf /usr/share/nginx/html && touch /var/run/nginx.pid && chown nginx:nginx /var/run/nginx.pid
COPY --from=builder /usr/src/app/dist /usr/share/nginx/html
COPY --from=builder /usr/src/app/nginx.conf /etc/nginx/conf.d/default.conf



USER nginx

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
