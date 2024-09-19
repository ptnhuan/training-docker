FROM node:16.17.0-alpine

ENV LANG=C.UTF-8 \
    APP_ROOT=/app

WORKDIR $APP_ROOT

RUN apk add tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    echo "Asia/Tokyo" > /etc/timezone && \
    apk del tzdata

RUN apk update && \
    apk upgrade && \
    apk add bash python3 make g++

RUN npm add -g @vue/cli @vue/cli-init

ENV HOST 0.0.0.0
EXPOSE 3000
