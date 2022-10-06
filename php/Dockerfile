FROM php:8.1-fpm

# ENV LD_LIBRARY_PATH /opt/oracle/instantclient_21_7
ENV ORACLE_HOME /opt/oracle/instantclient

RUN set -x \
        && groupmod -g 1000 www-data \
        && usermod -u 1000 -g 1000 www-data

RUN set -x \
        && apt-get update \
        && apt-get -y upgrade \
        && apt-get install --no-install-recommends -y \
        nginx \
        apt-transport-https \
        gnupg \
        && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
        && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
        && curl -sL https://deb.nodesource.com/setup_12.x | bash - \
        && apt-get install --no-install-recommends -y \
        git \
        libaio1 \
        libicu-dev \
        libaio-dev \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        nodejs \
        vim \
        unzip \
        yarn \
        bash \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

ADD instantclient-basic-linux.x64-21.7.0.0.0.zip /tmp/
ADD instantclient-sqlplus-linux.x64-21.7.0.0.0.zip /tmp/
ADD instantclient-sdk-linux.x64-21.7.0.0.0.zip /tmp/

RUN mkdir -p /opt/oracle/instantclient_21_7
RUN unzip /tmp/instantclient-basic-linux.x64-21.7.0.0.0.zip -d /opt/oracle/instantclient_21_7
RUN unzip /tmp/instantclient-sqlplus-linux.x64-21.7.0.0.0.zip -d  /opt/oracle/instantclient_21_7
RUN unzip /tmp/instantclient-sdk-linux.x64-21.7.0.0.0.zip -d  /opt/oracle/instantclient_21_7

RUN mv /opt/oracle/instantclient_21_7 /opt/oracle/instantclient

# RUN  set -x \
#         && chmod +x /opt/oracle/instantclient_21_7/sqlplus 
RUN ln -s /opt/oracle/instantclient_21_7/libclntsh.so.21.1 /opt/oracle/instantclient/libclntsh.so 
RUN ln -s /opt/oracle/instantclient_21_7/libclntshcore.so.21.1 /opt/oracle/instantclient/libclntshcore.so 
RUN ln -s /opt/oracle/instantclient_21_7/libocci.so.21.1 /opt/oracle/instantclient/libocci.so
RUN echo /opt/oracle/instantclient_21_7 > /etc/ld.so.conf.d/oracle-instantclient
ENV LD_LIBRARY_PATH=/opt/oracle/instantclient
RUN ldconfig

#RUN pecl install oci8

# RUN echo 'instantclient,/opt/oracle/instantclient' | pecl install oci8

# #RUN echo 'instantclient,/opt/oracle/instantclient' | pecl install oci8
# RUN docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient

# RUN docker-php-ext-configure intl
# RUN docker-php-ext-install pdo_oci
# RUN docker-php-ext-enable oci8


# RUN set -x \
#         && docker-php-ext-install -j$(nproc) \
#         bcmath \
#         intl \
#         gd \
#         pdo_oci \
#         zip

# RUN set -x \
#         && echo 'instantclient,/opt/oracle/instantclient/' | pecl install \
#         oci8-2.2.0 \
#         && yes '' | pecl install -f \
#         mcrypt

# RUN sed -i "s/user  nginx;/user  www-data;/g" /etc/nginx/nginx.conf                

# RUN set -x \
#         && docker-php-ext-enable \
#         mcrypt \
#         oci8 \
#         opcache

RUN set -x \
        && echo 'date.timezone = "Asia/Bangkok"' | tee /usr/local/etc/php/conf.d/locale.ini \
        && echo 'default_charset = "utf-8"' | tee -a /usr/local/etc/php/conf.d/locale.ini \
        && echo "memory_limit = 1024M" | tee /usr/local/etc/php/conf.d/memory-limit-php.ini \        
        && echo "max_execution_time = 1200" | tee -a /usr/local/etc/php/conf.d/memory-limit-php.ini \        
        && echo "max_input_time = 300" | tee -a /usr/local/etc/php/conf.d/memory-limit-php.ini \        
        && echo "upload_max_filesize = 512M" | tee /usr/local/etc/php/conf.d/file.ini \
        && echo "post_max_size = 512M" | tee -a /usr/local/etc/php/conf.d/file.ini 
