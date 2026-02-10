FROM php:8.4-fpm
RUN groupadd -r php && useradd -r -g php php
RUN apt-get update -y
RUN apt-get install -y openssl libssl-dev libcurl4-openssl-dev unzip git
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN pecl config-set php_ini "${PHP_INI_DIR}/php.ini"
RUN pecl install mongodb
RUN docker-php-ext-enable mongodb

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY composer.json composer.lock* /var/www/
COPY src/ /var/www/src/
WORKDIR /var/www
RUN composer install --ignore-platform-req=ext-mongodb --no-dev

# Copy src to the html serving directory as well
RUN cp -a /var/www/src/. /var/www/html/

USER php
