FROM php:7.0-fpm

MAINTAINER Mathieu A. <mathieu@shareyourscript.me>

ENV DEBIAN_FRONTEND noninteractive

ENV TZ=Europe/Paris
RUN echo $TZ | tee /etc/timezone
RUN dpkg-reconfigure --frontend noninteractive tzdata

COPY ./conf.d/php.ini /usr/local/etc/php/php.ini

RUN addgroup --system --gid 1008 wit
RUN useradd --system --create-home --shell /bin/bash --uid 1008 --gid wit wit

RUN apt-get update && apt-get upgrade -qq -y && apt-get install -qq -y \
        apt-utils \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libxslt-dev \
        zlib1g-dev \
        libicu-dev \
        libedit-dev \
        libcurl4-gnutls-dev \
        g++ \
        less \
        sed \
        git \
        cron \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install intl \
    && docker-php-ext-install xsl \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install readline \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install opcache \
    && docker-php-ext-install curl \
    && docker-php-source delete \
    && apt-get clean \
    && apt-get autoclean

RUN sed 's/user = www-data/user = wit/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed 's/group = www-data/group = wit/g' /usr/local/etc/php-fpm.d/www.conf

RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer \
    && chmod +x /usr/local/bin/composer

COPY ./cron/crontab /etc/crontab

EXPOSE 9000

#CMD /usr/sbin/cron
