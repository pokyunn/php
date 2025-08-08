FROM php:8.3.14-fpm-alpine3.20

# Install dependencies
RUN apk upgrade --update && apk add --no-cache \
    ca-certificates git findutils icu-dev \
    libxml2-dev libzip-dev libpng-dev \
    && docker-php-ext-install pdo_mysql xml soap zip gd intl bcmath

# Install PHP Extensions (igbinary & memcached)
RUN apk add --no-cache --update libmemcached-libs zlib

RUN set -xe && \
    cd /tmp/ && \
    apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS && \
    apk add --no-cache --update --virtual .memcached-deps zlib-dev libmemcached-dev cyrus-sasl-dev && \
# Install igbinary (memcached's deps)
    pecl install igbinary && \
# Install memcached
    ( \
        pecl install --nobuild memcached && \
        cd "$(pecl config-get temp_dir)/memcached" && \
        phpize && \
        ./configure --enable-memcached-igbinary && \
        make -j$(nproc) && \
        make install && \
        cd /tmp/ \
    ) && \
# Enable PHP extensions
    docker-php-ext-enable igbinary memcached && \
    rm -rf /tmp/* && \
    apk del .memcached-deps .phpize-deps

RUN apk add --no-cache gnu-libiconv

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
