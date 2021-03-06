FROM php:7.3.3-fpm-stretch

ARG PHALCON_VERSION=3.4.2
ARG PHALCON_EXT_PATH=php7/64bits
ARG XDEBUG_VERSION=2.7.0
ARG REDIS_VERSION=4.3.0

RUN apt-get update \
    && apt-get install -y zlib1g-dev libzip-dev unzip git libpng-dev libjpeg62-turbo-dev libxml2-dev \
    && docker-php-ext-install zip pdo_mysql gd soap bcmath \
    && docker-php-ext-configure bcmath --enable-bcmath \
    && docker-php-ext-configure zip --with-libzip \
    && pecl install xdebug-${XDEBUG_VERSION} \
    && docker-php-ext-enable xdebug \
    && pecl install redis-${REDIS_VERSION} \
    && docker-php-ext-enable redis \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/*

RUN set -xe && \
        curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
        tar xzf ${PWD}/v${PHALCON_VERSION}.tar.gz && \
        docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) ${PWD}/cphalcon-${PHALCON_VERSION}/build/${PHALCON_EXT_PATH} && \
        rm -r \
            ${PWD}/v${PHALCON_VERSION}.tar.gz \
            ${PWD}/cphalcon-${PHALCON_VERSION}

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

WORKDIR /code