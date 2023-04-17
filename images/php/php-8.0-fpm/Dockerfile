FROM php:8.0-fpm

#EXTENSIONS
RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        git-flow \
        bash-completion\
        vim \
        nano \
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

## Install Redis server and CLI
#RUN apt-get update && \
#    apt-get install -y redis-server redis-tools && \
#    rm -rf /var/lib/apt/lists/*

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

#INTERBASE FOR FIREBASE DB
RUN apt-get update && \
    apt-get install -y firebird-dev && \
    docker-php-ext-install pdo_firebird

#Redis
RUN apt-get update && \
    apt-get install -y redis-server

RUN apt-get install -y redis-tools

#Supervisor
RUN apt-get update && apt-get install -y supervisor

#PROJECT WORKDIR
WORKDIR /var/www

#PERMISSIONS WWW-DATA
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
RUN chown -R 1000:1000 /var/log/supervisor
USER 1000

#Enable git autocomplete
COPY bash.bashrc /etc

# CUSTOM PHP.INI CONFIG
COPY conf.d/* /usr/local/etc/php/conf.d/
#Supervisor conf
COPY etc/supervisord.conf /etc/supervisor/supervisord.conf
COPY etc/supervisor.d /etc/supervisor/supervisor.d/

#RUN CONTAINER
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]
#CMD ["php-fpm"]