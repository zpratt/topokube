FROM node:12.18.2-alpine as builder

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
ENV CI=true

COPY . .

RUN apk --no-cache --virtual build-dependencies add \
    python \
    make \
    g++ \
    && npm ci --no-optional --no-cache \
    && npm run build \
    && apk del build-dependencies

#######################################

FROM nginx:1.17.8-alpine

RUN mkdir -p /var/www/ui
WORKDIR /var/www/ui

COPY --from=builder /usr/src/app/out .

COPY --from=builder /usr/src/app/nginx.conf /etc/nginx/conf.d/default.conf
