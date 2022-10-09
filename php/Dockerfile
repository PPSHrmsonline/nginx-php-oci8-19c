FROM php:8.1-fpm
ENV ORACLE_HOME=/opt/oracle/instantclient

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
        wget \
        vim \
        unzip \
        yarn \
        bash \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*
# ADD instantclient-basic-linux.x64-21.7.0.0.0.zip /tmp/
# ADD instantclient-sqlplus-linux.x64-21.7.0.0.0.zip /tmp/
# ADD instantclient-sdk-linux.x64-21.7.0.0.0.zip /tmp/
 
RUN mkdir -p /opt/oracle
# RUN mkdir -p /opt/oracle/instantclient/sdk/include
RUN wget https://download.oracle.com/otn_software/linux/instantclient/217000/instantclient-basic-linux.x64-21.7.0.0.0dbru.zip  \ 
        && wget https://download.oracle.com/otn_software/linux/instantclient/217000/instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip \
        && wget https://download.oracle.com/otn_software/linux/instantclient/217000/instantclient-sqlplus-linux.x64-21.7.0.0.0dbru.zip \
        && unzip instantclient-basic-linux.x64-21.7.0.0.0dbru.zip -d /opt/oracle \
        && unzip instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip -d /opt/oracle \
        && unzip instantclient-sqlplus-linux.x64-21.7.0.0.0dbru.zip -d /opt/oracle \
        && rm -rf *.zip \
        && mv /opt/oracle/instantclient_21_7 /opt/oracle/instantclient

# RUN unzip /tmp/instantclient-sqlplus-linux.x64-21.7.0.0.0.zip -d  /opt/oracle
# RUN unzip /tmp/instantclient-sdk-linux.x64-21.7.0.0.0.zip -d  /opt/oracle
 #Update the Dynamic Linker Run-Time Bindings -->ldconfig


RUN echo /opt/oracle/instantclient/ > /etc/ld.so.conf.d/oic.conf && \
    ldconfig 
ENV LD_LIBRARY_PATH=instantclient,/opt/oracle/instantclient
  
RUN ln -s /opt/oracle/instantclient_21_7 /opt/oracle/instantclient
RUN docker-php-ext-configure oci8  --with-oci8=instantclient,/opt/oracle/instantclient \
        && docker-php-ext-install oci8

RUN docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient
RUN docker-php-ext-install pdo_mysql zip  exif pcntl 
#RUN docker-php-ext-install  mcrypt

RUN docker-php-ext-install bcmath  
RUN docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient,21.7
RUN docker-php-ext-install pdo_oci 
RUN docker-php-ext-install intl gd zip 
 
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
