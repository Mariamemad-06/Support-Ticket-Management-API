FROM php:8.3-fpm

WORKDIR /var/www/html

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        unzip \
        libzip-dev \
    && docker-php-ext-install pdo pdo_mysql zip \
    && pecl install redis && docker-php-ext-enable redis \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
RUN useradd \
        --uid 1000 \
        --gid www-data \
        --groups www-data,root \
        --create-home \
        --home-dir /home/laravel \
        laravel

RUN chown -R laravel:www-data /var/www/html /home/laravel

COPY --chown=laravel:www-data . .
USER laravel
EXPOSE 9000
CMD ["php-fpm"]
