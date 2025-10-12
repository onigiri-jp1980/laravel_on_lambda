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
RUN apk update && \
    apk add libpq-dev jq nodejs npm && \
    npm install && \
    npm install -g serverless@v3
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]

FROM base as deploy