FROM php:8-fpm-alpine

RUN apk update && \
    apk add curl bash git php8-mbstring php8-json php8-xml php8-pdo_dblib php8-pdo_odbc \
    php8-pdo_pgsql php8-pgsql \
    php8-intl php8-opcache \
    php8-mysqli php8-pdo_mysql php8-pdo_sqlite

COPY php.ini /etc/php/8/cli/php.ini
COPY php.ini /etc/php/8/fpm/php.ini

WORKDIR /app

RUN addgroup -S backend && adduser -S backend -G backend

RUN curl https://get.symfony.com/cli/installer | bash

RUN mv ~/.symfony/bin/symfony /usr/local/bin/symfony

COPY --from=composer /usr/bin/composer /usr/bin/composer

EXPOSE 8088

USER backend

CMD ["symfony","serve","--dir=.","--no-tls","--allow-http","--document-root=public/","--port=8088","-q"]