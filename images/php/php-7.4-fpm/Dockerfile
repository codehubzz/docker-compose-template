FROM arodax/php7.4-fpm

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
        && pecl install mcrypt-1.0.3 \
	    && docker-php-ext-enable mcrypt \
        && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip sockets \
	    && docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd


#PHP REDIS
#RUN pecl install redis && docker-php-ext-enable redis

#COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#NODE JS VERSION
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs

#PROJECT WORKDIR
WORKDIR /var/www

#PERMISSIONS WWW-DATA
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
USER 1000

# CUSTOM PHP.INI CONFIG
ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

#RUN CONTAINER
CMD ["php-fpm"]