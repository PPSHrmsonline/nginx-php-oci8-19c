FROM php:fpm

ENV LD_LIBRARY_PATH /opt/oracle/instantclient_19_10
ENV ORACLE_HOME /opt/oracle/instantclient_19_10

RUN set -x \
        && apk --no-cache add shadow \
        && groupmod -g 1000 www-data \
        && useradd -u 1000 -g 1000 www-data

RUN set -x \
        && apk add \
               apt-transport-https \
               gnupg \
        && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
        && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
        && curl -sL https://deb.nodesource.com/setup_9.x | bash - \
        && apk add \
                git \
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
                bash \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

ADD instantclient_19_10.tar.gz /opt/oracle/
RUN  set -x \
        && chmod +x /opt/oracle/instantclient_19_10/sqlplus \
        && ln -s /opt/oracle/instantclient_19_10/libclntsh.so.19.1 /opt/oracle/instantclient_19_10/libclntsh.so \
        && ln -s /opt/oracle/instantclient_19_10/libclntshcore.so.19.1 /opt/oracle/instantclient_19_10/libclntshcore.so \
        && ln -s /opt/oracle/instantclient_19_10/libocci.so.19.1 /opt/oracle/instantclient_19_10/libocci.so

RUN set -x \
        && docker-php-ext-configure \
                pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_19_10/,19.10 \
        && docker-php-ext-configure \
                intl \
        && docker-php-ext-install -j$(nproc) \
                bcmath \
                intl \
                gd \
                pdo_oci \
                zip

RUN set -x \
        && echo 'instantclient,/opt/oracle/instantclient_19_10/' | pecl install \
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
