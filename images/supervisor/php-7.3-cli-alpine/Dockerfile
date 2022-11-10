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
RUN apk --no-cache add supervisor bash tzdata shadow

#TIMEZONE
ENV TZ=${TZ}

# CUSTOM PHP.INI CONFIG
COPY conf.d/* /usr/local/etc/php/conf.d/

#PERMISSIONS WWW-DATA
#setting same uid as in php container (1000)
#default www-data uid is 82 in alpine
RUN deluser www-data
RUN /usr/sbin/groupadd -g 1000 www-data
RUN /usr/sbin/useradd -s /bin/sh -g 1000 -u 1000 www-data
USER 1000

WORKDIR /var/www
