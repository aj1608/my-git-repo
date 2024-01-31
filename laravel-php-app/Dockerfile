FROM php:8.2-apache

RUN apt-get update && \
    apt-get install -y \
        libzip-dev \
        unzip && \
    docker-php-ext-install zip && \
    rm -rf /var/lib/apt/lists/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer clear-cache

WORKDIR /var/www/html

COPY laravel-php-app/ .

RUN composer install --no-scripts --no-interaction && \
    php artisan key:generate

ENV APACHE_DOCUMENT_ROOT /mnt/c/akshaya/my-laravel-project/laravel-php-app/public/
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite && service apache2 restart

EXPOSE 80

CMD ["apache2-foreground"]

