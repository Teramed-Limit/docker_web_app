FROM node:alpine as builder

WORKDIR /app
COPY package.json package-lock.json ./
ENV CI=1
RUN npm ci
COPY . .
RUN npm run build

# Deploy our react app to NGINX
FROM nginx:alpine

## Replace the default nginx index page with our react app
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder ./app/build /usr/share/nginx/html
#COPY build /usr/share/nginx/html

## Copy ssl certificate
COPY ./nginx/ssl/server.crt /etc/nginx/ssl/server.crt
COPY ./nginx/ssl/server.key /etc/nginx/ssl/server.key

COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./nginx/gzip.conf /etc/nginx/gzip.conf

ENTRYPOINT ["nginx", "-g", "daemon off;"]