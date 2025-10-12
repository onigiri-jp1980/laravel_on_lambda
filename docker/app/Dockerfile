FROM php:8.4-fpm-alpine as base

WORKDIR /app
ARG PHP_PREFIX=php

COPY app/ .

#基本セットアップ
RUN apk add ${PHP_PREFIX}-pdo ${PHP_PREFIX}-pdo_mysql ${PHP_PREFIX}-pdo_pgsql unzip

#composerインストール
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#laravelのインストール
RUN composer install && \
    composer dump-autoload && \
    php artisan key:generate && \
    php artisan migrate

CMD ["php-fpm"]

FROM base as local
ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY
ARG AWS_DEFAULT_REGION
ARG AWS_BUCKET
ARG AWS_ENDPOINT
ARG AWS_USE_PATH_STYLE_ENDPOINT

RUN apk update && \
    apk add libpq-dev jq nodejs npm aws-cli && \
    npm install && \
    npm install -g serverless@v3 && \
    php artisan migrate:refresh
COPY ./docker/app/aws/credentials /root/.aws/credentials
COPY ./docker/app/aws/config /root/.aws/config

CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]

FROM base as deploy