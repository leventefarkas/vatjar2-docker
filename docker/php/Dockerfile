FROM php:7.2.9-fpm-stretch

ENV PHP_EXTRA_CONFIGURE_ARGS --enable-maintainer-zts
ARG PHALCON_VERSION=3.4.2
ARG PHALCON_EXT_PATH=php7/64bits

RUN apt update \
    && apt install -y zlib1g-dev unzip git libpng-dev libjpeg62-turbo-dev \
    && docker-php-ext-install zip pdo_mysql gd \
    && pecl install xdebug-2.6.1 \
    && docker-php-ext-enable xdebug \
#   && pecl install pthreads \
#   && docker-php-ext-enable pthreads \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/*

RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
    echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini;

RUN set -xe && \
        curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
        tar xzf ${PWD}/v${PHALCON_VERSION}.tar.gz && \
        docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) ${PWD}/cphalcon-${PHALCON_VERSION}/build/${PHALCON_EXT_PATH} && \
        rm -r \
            ${PWD}/v${PHALCON_VERSION}.tar.gz \
            ${PWD}/cphalcon-${PHALCON_VERSION}

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

WORKDIR /code