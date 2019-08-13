FROM php:7.3-fpm-alpine

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo --no-plugins --no-scripts

# Install dependencies
RUN apk upgrade --update && apk add --no-cache \
    libxml2-dev libzip-dev \
    && docker-php-ext-install pdo_mysql xml soap zip

COPY ./docker.conf /usr/local/etc/php-fpm.d/docker.conf

# Create code dir
RUN mkdir /code 

WORKDIR /code
