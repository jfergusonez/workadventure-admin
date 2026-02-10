FROM php:8.4-fpm

RUN groupadd -r php && useradd -r -g php php

RUN apt-get update -y
RUN apt-get install -y openssl libssl-dev libcurl4-openssl-dev
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN pecl config-set php_ini "${PHP_INI_DIR}/php.ini"
RUN pecl install mongodb
RUN docker-php-ext-enable mongodb

# Install composer and dependencies
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY composer.json composer.lock* /var/www/
WORKDIR /var/www
RUN composer install --ignore-platform-req=ext-mongodb --no-dev

# Copy app source
COPY src/ /var/www/html/

USER php
