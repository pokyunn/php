FROM php:7.2-fpm-alpine

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo --no-plugins --no-scripts

# Install dependencies
RUN apk upgrade --update && apk add --no-cache \
    libxml2-dev \
    && docker-php-ext-install pdo_mysql xml soap
