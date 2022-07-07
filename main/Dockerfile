FROM debian:10
RUN apt-get update
RUN apt-get install software-properties-common -y
RUN apt -y install lsb-release apt-transport-https ca-certificates wget

RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" |  tee /etc/apt/sources.list.d/php.list
RUN apt update -y && apt upgrade -y
#RUN add-apt-repository ppa:ondrej/php && apt update -y 


ENV LD_LIBRARY_PATH /opt/oracle/instantclient_19_3
ENV ORACLE_HOME /opt/oracle/instantclient_19_3
RUN mkdir -p /opt/oracle
RUN apt-get update && apt-get install -y \
    software-properties-common\
    apt-transport-https\
    gnupg\
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
    unzip \
    && rm -rf /var/lib/apt/lists/*

#####Installation PHP-FPM################################



RUN set -x \
    apt-get install -y\
    php7.4-fpm\
    php7.4-bcmath\
    php7.4-curl\
    php7.4-gd\
    php7.4-mbstring\
    php7.4-mysql\
    php7.4-xml\
    php7.4-zip\
    php-pear\
    php-intl

####RUN PHP Version#########



ADD instantclient_19_3.tar.gz /opt/oracle/
RUN  set -x \
    && chmod +x /opt/oracle/instantclient_19_3/sqlplus \
    && ln -s /opt/oracle/instantclient_19_3/libclntsh.so.19.1 /opt/oracle/instantclient_19_3/libclntsh.so \
    && ln -s /opt/oracle/instantclient_19_3/libclntshcore.so.19.1 /opt/oracle/instantclient_19_3/libclntshcore.so \
    && ln -s /opt/oracle/instantclient_19_3/libocci.so.19.1 /opt/oracle/instantclient_19_3/libocci.so

# RUN set -x \
#     && docker-php-ext-configure \
#     pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_19_3/,19.3 \


#     && docker-php-ext-install -j$(nproc) \

#     pdo_oci \



# RUN wget https://pecl.php.net/get/PDO_OCI-1.0.tgz
# RUN tar -xvf PDO_OCI-1.0.tgz
# RUN PDO_OCI-1.0



# RUN set -x \
#     && docker-php-ext-enable \
#     mcrypt \
#     oci8 \
#     opcache

# RUN set -x \
#     && echo 'date.timezone = "Asia/Bangkok"' | tee /etc/php/conf.d/locale.ini \
# && echo 'default_charset = "utf-8"' | tee -a /usr/local/etc/php/conf.d/locale.ini \
# && echo "memory_limit = 1024M" | tee /usr/local/etc/php/conf.d/memory-limit-php.ini \        
# && echo "max_execution_time = 1200" | tee -a /usr/local/etc/php/conf.d/memory-limit-php.ini \        
# && echo "max_input_time = 300" | tee -a /usr/local/etc/php/conf.d/memory-limit-php.ini \        
# && echo "upload_max_filesize = 512M" | tee /usr/local/etc/php/conf.d/file.ini \
# && echo "post_max_size = 512M" | tee -a /usr/local/etc/php/conf.d/file.ini 



