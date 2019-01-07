FROM php:7.2-fpm-alpine
MAINTAINER Dmitriy Rudenskiy <dmitriy.rudenskiy@gmail.com>

RUN set -xe && apk add --update --no-cache git openssh-client mariadb-client

RUN set -ex; \
    apk add --no-cache --virtual .build-deps \
    bzip2-dev \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libxpm-dev; \
    docker-php-ext-configure gd --with-freetype-dir=/usr --with-jpeg-dir=/usr --with-webp-dir=/usr --with-png-dir=/usr --with-xpm-dir=/usr; \
    docker-php-ext-install bz2 gd mysqli opcache zip pdo pdo_mysql; \
    runDeps="$( \
    scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
    | tr ',' '\n' \
    | sort -u \
    | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )"; \
    apk add --virtual .phpmyadmin-phpexts-rundeps $runDeps; \
    apk del .build-deps


RUN set -xe; \
    curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar; \
    chmod +x wp-cli.phar; \
    mv wp-cli.phar /usr/local/bin/wp

# Config php
RUN echo 'sendmail_path = /usr/sbin/sendmail -S mail:1025' > /usr/local/etc/php/php.ini \
    && echo "memory_limit=-1" >> /usr/local/etc/php/php.ini \
    && echo "date.timezone=${PHP_TIMEZONE:-UTC}" >> /usr/local/etc/php/php.ini

# Setup the Composer installer
RUN curl --silent --fail --location --retry 3 --output /tmp/composer-setup.php --url https://getcomposer.org/installer \
     && php /tmp/composer-setup.php --no-ansi --install-dir=/usr/bin --filename=composer \
     && rm -rf /composer-setup.php