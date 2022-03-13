# nginx 1.20.2 is based on alpine 3.14

FROM nginx:1.20.2-alpine

ARG ALPINE_VERSION=v3.14
ARG NODEJS_VERSION=14.19.0-r0
ARG NPM_VERSION=7.17.0-r0
ARG YARN_VERSION=1.22.10-r0

LABEL maintainer="Jordi Sabater"

# Install nvm with node and npm
RUN apk add --no-cache --repository http://nl.alpinelinux.org/alpine/$ALPINE_VERSION/releases/ libuv \
    && apk add --no-cache --update-cache --repository http://nl.alpinelinux.org/alpine/$ALPINE_VERSION/main nodejs=$NODEJS_VERSION npm=$NPM_VERSION \
    && apk add --no-cache --update-cache --repository http://nl.alpinelinux.org/alpine/$ALPINE_VERSION/community yarn=$YARN_VERSION \
    && echo "NodeJS Version:" "$(node -v)" \
    && echo "NPM Version:" "$(npm -v)" \
    && echo "Yarn Version:" "$(yarn -v)" \