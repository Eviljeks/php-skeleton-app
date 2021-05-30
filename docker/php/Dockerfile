FROM php:8-fpm-alpine

ARG UID
ARG GID

RUN set -ex; \
    \
    apk add --no-cache --virtual .build-deps \
        postgresql-dev \
    ; \
    docker-php-ext-configure pgsql \
        -with-pgsql=/usr/include/postgresql/ \
    ; \
    docker-php-ext-install \
        pdo \
        pdo_pgsql \
        pgsql  \
    ; \
    pecl install xdebug &&  docker-php-ext-enable xdebug \
    ; \
    apk --update --no-cache add \
    php8-mbstring \
    php8-json \
    php8-xml \
    php8-intl \
    php8-opcache \
    php8-openssl \
    php8-simplexml \
    php8-fileinfo \
    php8-xmlwriter \
    php8-gd \
    sudo bash git supervisor nginx curl bash openssl ca-certificates

COPY php.ini /usr/local/etc/php/php.ini
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN apk --no-cache add pcre-dev ${PHPIZE_DEPS} \ 
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && apk del pcre-dev ${PHPIZE_DEPS}

WORKDIR /app

RUN addgroup -S backend -g ${GID} && adduser -S -G backend backend -u ${UID}

RUN curl https://get.symfony.com/cli/installer | bash

RUN mv ~/.symfony/bin/symfony /usr/local/bin/symfony

COPY --from=composer /usr/bin/composer /usr/bin/composer

USER backend