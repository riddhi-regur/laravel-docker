# FROM php:8.4-fpm

# RUN apt-get update && apt-get install -y \
#     git curl zip unzip libpq-dev

# RUN docker-php-ext-install pdo pdo_pgsql

# COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# WORKDIR /var/www

FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    git unzip curl libpq-dev libonig-dev libxml2-dev zip \
    && docker-php-ext-install pdo pdo_pgsql mbstring xml ctype fileinfo

RUN a2enmod rewrite

WORKDIR /var/www/html

COPY . .

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN composer install --no-dev --optimize-autoloader --no-interaction

RUN chown -R www-data:www-data storage bootstrap/cache

ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
