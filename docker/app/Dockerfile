FROM php:8.4-fpm-alpine

WORKDIR /app

COPY . .

RUN docker-php-ext-install pdo pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

CMD ["php-fpm"]