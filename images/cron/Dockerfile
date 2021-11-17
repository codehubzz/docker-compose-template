FROM php:7.4-fpm-alpine

#redis
RUN apk --no-cache add pcre-dev ${PHPIZE_DEPS} \
      && pecl install redis \
      && docker-php-ext-enable redis \
      && apk del pcre-dev ${PHPIZE_DEPS} \
      && rm -rf /tmp/pear

#pdo
RUN  docker-php-ext-install pdo pdo_mysql

# Указываем рабочую директорию для PHP
WORKDIR /var/www

COPY config/cronjobs /etc/crontabs/root

CMD ["crond", "-f", "-d", "8"]