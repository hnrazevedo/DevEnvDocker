FROM php:8.0.2-fpm

MAINTAINER Henri Azevedo <hnr.azevedo@gmail.com>

ARG INSTALL_GD=false
ARG INSTALL_COMPRESSION=false
ARG INSTALL_FTP=false
ARG INSTALL_MCRYPT=false
ARG INSTALL_PDO_MYSQL=false
ARG INSTALL_PDO_POSTGRESQL=false
ARG INSTALL_MBSTRING=false
ARG INSTALL_BCMATH=false
ARG INSTALL_OPCACHE=false
ARG INSTALL_MYSQLI=false
ARG INSTALL_XDEBUG=false
ARG INSTALL_COMPOSER=false
ARG INSTALL_TOKENIZER=false
ARG INSTALL_JSON_XML=false
ARG INSTALL_CURL=false
ARG INSTALL_OPENSSL=false
ARG INSTALL_INTL=false

# Additional tools
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
     curl \
     openssl \
     libssl-dev 
# Common php-ext and requirements
RUN apt-get install -y --no-install-recommends libpq-dev libz-dev \
  && docker-php-ext-install pcntl \
  && docker-php-ext-install session \
  && docker-php-ext-install phar \
  && docker-php-ext-install iconv \
  && docker-php-ext-install pdo 

#####################################
# GD:
#####################################

RUN if [ ${INSTALL_GD} = true ]; then \
    # Install gd and requirements
    apt-get install -y --no-install-recommends libjpeg62-turbo-dev libpng-dev libfreetype6-dev \
&& docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install gd \
;fi

#####################################
# compression:
#####################################

RUN if [ ${INSTALL_COMPRESSION} = true ]; then \
    # Install zip, bz2 and requirements
    apt-get install -y --no-install-recommends libbz2-dev zlib1g-dev libzip-dev \
 && docker-php-ext-install zip \
    bz2 \
;fi

#####################################
# FTP:
#####################################

RUN if [ ${INSTALL_FTP} = true ]; then \
    # Install ftp and requirements
    apt-get install -y --no-install-recommends libssl-dev \
 && docker-php-ext-install ftp \
;fi

#####################################
# MCrypt:
#####################################

RUN if [ ${INSTALL_MCRYPT} = true ]; then \
    # Install mcrypt and requirements
    apt-get install -y --no-install-recommends libmcrypt-dev && \
# && docker-php-ext-install mcrypt \
 pecl install mcrypt-1.0.5 \
&& docker-php-ext-enable mcrypt \
;fi

#####################################
# Mbstring:
#####################################

RUN if [ ${INSTALL_MBSTRING} = true ]; then \
    # Install mbstring
    apt-get install libonig-dev && \
    docker-php-ext-install mbstring \
;fi

#####################################
# PDO_MYSQL:
#####################################

RUN if [ ${INSTALL_PDO_MYSQL} = true ]; then \
    # Install pdo_mysql
    docker-php-ext-install pdo_mysql \
;fi

#####################################
# PDO_POSTGRESQL:
#####################################

RUN if [ ${INSTALL_PDO_POSTGRESQL} = true ]; then \
    # Install pdo_pgsql
    docker-php-ext-install pdo_pgsql \
;fi

#####################################
# Mysqli Modifications:
#####################################

RUN if [ ${INSTALL_MYSQLI} = true ]; then \
    # Install mysqli
    docker-php-ext-install mysqli \
;fi

#####################################
# bcmath:
#####################################

RUN if [ ${INSTALL_BCMATH} = true ]; then \
    # Install bcmath extension
    docker-php-ext-install bcmath \
;fi

#####################################
# Opcache:
#####################################

RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

#####################################
# xdebug:
#####################################

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    # Install the xdebug
    pecl install xdebug \
    docker-php-ext-enable xdebug \
;fi

# Copy xdebug configration
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#####################################
# composer:
#####################################

RUN if [ ${INSTALL_COMPOSER} = true ]; then \
    # Install the composer
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
;fi

# Copy xdebug configration
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#####################################
# tokenizer:
#####################################

RUN if [ ${INSTALL_TOKENIZER} = true ]; then \
    # Install tokenizer
    docker-php-ext-install tokenizer \
;fi

#####################################
# json, xml, dom xmlrpc, xsl:
#####################################

RUN if [ ${INSTALL_JSON_XML} = true ]; then \
    # Install xml, dom xmlrpc, xsl, and requirements
    apt-get install -y --no-install-recommends libxml2-dev libxslt-dev \
 && docker-php-ext-install \
# json \
    xml \
    dom \
#    xmlrpc \
    xsl \
;fi

#####################################
# openssl:
#####################################

#ARG INSTALL_OPENSSL=false
#RUN if [ ${INSTALL_OPENSSL} = true ]; then \
#    # Install openssl
#    apt-get install -y --no-install-recommends openssl-dev \
#    docker-php-ext-install openssl \
#;fi

#####################################
# curl:
#####################################

#ARG INSTALL_CURL=false
#RUN if [ ${INSTALL_CURL} = true ]; then \
#    # Install curl and requirements
#    apt-get install -y --no-install-recommends libcurl3 libcurl3-dev \
#    docker-php-ext-install curl \
#;fi

#####################################
# Human Language and Character Encoding Support:
#####################################

RUN if [ ${INSTALL_INTL} = true ]; then \
    # Install intl and requirements
    apt-get install -y --no-install-recommends zlib1g-dev libicu-dev g++ \
 && docker-php-ext-configure intl \
 && docker-php-ext-install intl \
;fi

# Clear package lists
RUN rm -rf /var/lib/apt/lists/*

# Permissions
RUN chown -R root:www-data /var/www/html
RUN chmod u+rwx,g+rx,o+rx /var/www/html
RUN find /var/www/html -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find /var/www/html -type f -exec chmod u+rw,g+rw,o+r {} +

CMD ["php-fpm"]

EXPOSE 9000
