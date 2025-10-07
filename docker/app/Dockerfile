FROM php:8.4-fpm-alpine as base

WORKDIR /app

COPY app/ .

RUN docker-php-ext-install pdo pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN composer install && \
    composer dump-autoload && \
    php artisan key:generate && \
    php artisan migrate

CMD ["php-fpm"]

FROM base as deployment

