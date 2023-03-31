FROM php:7.3-fpm

#EXTENSIONS
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libonig-dev \
        libzip-dev \
        libmcrypt-dev
RUN apt-get update && apt-get install -y \
        && pecl install mcrypt-1.0.3 \
	    && docker-php-ext-enable mcrypt \
        && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip sockets opcache\
	    #&& docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd \
     #    Чистим временные файлы
        && docker-php-source delete \
        && apt-get autoremove --purge -y && apt-get autoclean -y && apt-get clean -y

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

#INTERBASE FOR FIREBASE DB
RUN apt-get -y install firebird-dev
RUN docker-php-ext-install interbase
RUN docker-php-ext-enable interbase

#PHP REDIS
RUN pecl install redis && docker-php-ext-enable redis

#COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configure LDAP.
RUN apt-get update \
 && apt-get install libldap2-dev -y \
 && rm -rf /var/lib/apt/lists/* \
 && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
 && docker-php-ext-install ldap

#NODE JS
ARG NODE_VER
RUN if [ "$NODE_VER" != "" ];\
  then echo 'Installation of NODE_VER '${NODE_VER};\
    curl -sL https://deb.nodesource.com/setup_${NODE_VER}.x | bash - \
    && apt-get install -y nodejs  ;\
  else echo 'NODE_VER empty in env. Installation ver: latest';  \
    apt-get update && apt-get install -y \
    nodejs \
    npm ;\
  fi

#EXTENSIONS
RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        git-flow \
        bash-completion\
        mc\
        vim \
        nano

#PROJECT WORKDIR
WORKDIR /var/www

#PERMISSIONS WWW-DATA
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
USER 1000

#Enable git autocomplete
COPY bash.bashrc /etc

# CUSTOM PHP.INI CONFIG
COPY conf.d/* /usr/local/etc/php/conf.d/

#RUN CONTAINER
CMD ["php-fpm"]