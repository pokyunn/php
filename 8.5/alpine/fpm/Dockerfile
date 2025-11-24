FROM php:8.5-fpm-alpine

# Install dependencies
RUN apk upgrade --update \
    && apk add --no-cache \
        ca-certificates \
        findutils \
        git \
        gmp-dev \
        gnu-libiconv \
        icu-data-full \
        icu-dev \
        libpng-dev \
        libxml2-dev \
        libzip-dev \
        oniguruma-dev \
        p7zip \
        zlib \
    && docker-php-ext-install -j$(nproc) \
        bcmath \
        bz2 \
        gd \
        gmp \
        intl \
        mbstring \
        pdo_mysql \
        soap \
        xml \
        zip

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

RUN sed -i \
    -e 's/^;\(date.timezone =.*\)/date.timezone = UTC/' \
    -e 's/^\(max_execution_time =\).*/\1 120/' \
    -e 's/^\(max_input_time =\).*/\1 120/' \
    -e 's/^\(memory_limit =\).*/\1 1G/' \
    -e 's/^\(upload_max_filesize =\).*/\1 50M/' \
    -e 's/^\(post_max_size =\).*/\1 50M/' \
    -e 's/^\(error_reporting =\).*/\1 E_ALL/' \
    /usr/local/etc/php/php.ini

COPY ./docker.conf /usr/local/etc/php-fpm.d/docker.conf
RUN sed -i \
    -e 's/^\(user = www-data\)/;\1/' \
    -e 's/^\(group = www-data\)/;\1/' \
    /usr/local/etc/php-fpm.d/www.conf

RUN set -eux; \
    addgroup -g 1000 -S host-user; \
    adduser -u 1000 -D -S -G host-user host-user

# Create code dir
RUN mkdir /code && chown host-user: /code

USER host-user

WORKDIR /code
