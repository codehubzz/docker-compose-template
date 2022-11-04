FROM php:7.4-cli-alpine

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && pecl install uploadprogress \
    && docker-php-ext-enable uploadprogress \
    && apk del .build-deps $PHPIZE_DEPS \
    && chmod uga+x /usr/local/bin/install-php-extensions && sync \
    && install-php-extensions \
      gd xml zip curl mcrypt iconv mbstring sockets opcache redis \
      mysqli pgsql pdo pdo_mysql pdo_pgsql

RUN apk --no-cache add bash tzdata
ENV TZ=${TZ}

#PHP WORKDIR
WORKDIR /var/www

COPY /config/cronjobs /etc/crontabs/root