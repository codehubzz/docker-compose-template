FROM php:8.0-fpm

#EXTENSIONS
RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        git-flow \
        vim \
        nano \
#        nodejs \
#        npm \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libonig-dev \
        libzip-dev \
        libmcrypt-dev \
#        && pecl install mcrypt-1.0.3 \
#	    && docker-php-ext-enable mcrypt \
        && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip sockets opcache \
	    && docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd \
    #    Чистим временные файлы
        && docker-php-source delete \
        && apt-get autoremove --purge -y && apt-get autoclean -y && apt-get clean -y


#PHP REDIS
#RUN pecl install redis && docker-php-ext-enable redis

#COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#NODE JS VERSION
ARG NODE_VER
RUN curl -sL https://deb.nodesource.com/setup_${NODE_VER}.x | bash -
RUN apt-get install -y nodejs

#PROJECT WORKDIR
WORKDIR /var/www

#PERMISSIONS WWW-DATA
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
USER 1000

# CUSTOM PHP.INI CONFIG
COPY conf.d/* /usr/local/etc/php/conf.d/

#RUN CONTAINER
CMD ["php-fpm"]