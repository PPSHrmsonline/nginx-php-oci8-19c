FROM php:fpm

ENV LD_LIBRARY_PATH /opt/oracle/instantclient_19_3
ENV ORACLE_HOME /opt/oracle/instantclient_19_3

RUN set -x \
        && groupmod -g 1000 www-data \
        && usermod -u 1000 www-data

RUN set -x \
        && apt-get update \
        && apt-get -y upgrade \
        && apt-get install --no-install-recommends -y \
               apt-transport-https \
               gnupg \
        && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
        && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
        && curl -sL https://deb.nodesource.com/setup_9.x | bash - \
        && apt-get install --no-install-recommends -y \
                git \
                bash \
                libaio1 \
                libicu-dev \
                libjpeg62-turbo-dev \
                libfreetype6-dev \
                libmcrypt-dev \
                libpng-dev \
                libzip-dev \
                nodejs \
                vim \
                yarn \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

ADD instantclient_19_3.tar.gz /opt/oracle/
RUN  set -x \
        && chmod +x /opt/oracle/instantclient_19_3/sqlplus \
        && ln -s /opt/oracle/instantclient_19_3/libclntsh.so.19.1 /opt/oracle/instantclient_19_3/libclntsh.so \
        && ln -s /opt/oracle/instantclient_19_3/libclntshcore.so.19.1 /opt/oracle/instantclient_19_3/libclntshcore.so \
        && ln -s /opt/oracle/instantclient_19_3/libocci.so.19.1 /opt/oracle/instantclient_19_3/libocci.so

RUN set -x \
        && docker-php-ext-configure \
                pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_19_3/,19.3 \
        && docker-php-ext-install -j$(nproc) \
                gd \
                pdo_oci \
                zip \
                zlib

RUN set -x \
        && echo 'instantclient,/opt/oracle/instantclient_19_3/' | pecl install \
                oci8 \
        && yes '' | pecl install -f \
                mcrypt

RUN set -x \
        && docker-php-ext-enable \
                mcrypt \
                oci8 \
                opcache

RUN set -x \
        && echo 'date.timezone = "Asia/Bangkok"' | tee /usr/local/etc/php/conf.d/locale.ini \
        && echo 'default_charset = "utf-8"' | tee -a /usr/local/etc/php/conf.d/locale.ini \
        && echo "memory_limit = 1024M" | tee /usr/local/etc/php/conf.d/memory-limit-php.ini \        
        && echo "max_execution_time = 1200" | tee -a /usr/local/etc/php/conf.d/memory-limit-php.ini \        
        && echo "max_input_time = 300" | tee -a /usr/local/etc/php/conf.d/memory-limit-php.ini \        
        && echo "upload_max_filesize = 512M" | tee /usr/local/etc/php/conf.d/file.ini \
        && echo "post_max_size = 512M" | tee -a /usr/local/etc/php/conf.d/file.ini 
