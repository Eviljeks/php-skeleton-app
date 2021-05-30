FROM php:8-fpm-alpine

ARG UID
ARG GID

RUN apk --update --no-cache add \
    php8-mbstring \
    php8-json \
    php8-xml \
    php8-pdo_dblib \
    php8-pdo_odbc \
    php8-pdo_pgsql \
    php8-pgsql \
    php8-mysqli \
    php8-pdo_mysql \
    php8-pdo_sqlite \
    php8-intl \
    php8-opcache \
    php8-openssl \
    php8-simplexml \
    php8-fileinfo \
    php8-xmlwriter \
    php8-gd \
    sudo bash git supervisor nginx curl bash openssl ca-certificates

COPY php.ini /etc/php/8/cli/php.ini
COPY php.ini /etc/php/8/fpm/php.ini

WORKDIR /app

RUN addgroup -S backend -g ${GID} && adduser -S -G backend backend -u ${UID}

RUN curl https://get.symfony.com/cli/installer | bash

RUN mv ~/.symfony/bin/symfony /usr/local/bin/symfony

COPY --from=composer /usr/bin/composer /usr/bin/composer

USER backend