FROM php:7.3-cli-alpine

#PHP DEPENDENCIES
#ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/firebird-3/install-php-extensions /usr/local/bin/
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && apk del .build-deps $PHPIZE_DEPS \
    && chmod uga+x /usr/local/bin/install-php-extensions && sync \
    && install-php-extensions \
      gd xml zip curl mcrypt iconv mbstring sockets opcache redis \
      mysqli pgsql pdo pdo_mysql pdo_pgsql \
      interbase \
    && docker-php-source delete

#ANOTHER EXTENSIONS
RUN apk --no-cache add bash tzdata

#TIMEZONE
ENV TZ=${TZ}

# CUSTOM PHP.INI CONFIG
COPY conf.d/* /usr/local/etc/php/conf.d/

#CRONJOBS
COPY /config/cronjobs /etc/crontabs/root

#PHP WORKDIR
WORKDIR /var/www