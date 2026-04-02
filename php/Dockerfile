# FROM php:8.4-fpm

# RUN apt-get update && apt-get install -y \
#     git curl zip unzip libpq-dev

# RUN docker-php-ext-install pdo pdo_pgsql

# COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# WORKDIR /var/www

FROM php:8.3-apache

RUN apt-get update && apt-get install -y \
    git unzip curl libpq-dev libonig-dev libxml2-dev zip \
    && docker-php-ext-install pdo pdo_pgsql mbstring xml ctype fileinfo bcmath exif pcntl

RUN a2enmod rewrite

WORKDIR /var/www/html

COPY . .

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN cp .env.example .env || true

RUN composer install --no-dev --optimize-autoloader --no-interaction --no-scripts --ignore-platform-reqs

RUN chown -R www-data:www-data storage bootstrap/cache
RUN chmod -R 775 storage bootstrap/cache

ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
