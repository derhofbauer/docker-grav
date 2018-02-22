FROM php:7-apache
MAINTAINER Alexander Hofbauer <hofbauer.alexander@gmail.com>

ENV COMPOSER_ALLOW_SUPERUSER 1

#### Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

## Install stuff
RUN apt-get update
RUN apt-get install -y --no-install-recommends \
        git-core curl libxml2-dev libjpeg-dev libpng-dev libxpm-dev libxml2-dev libfreetype6-dev openssl && \
## Enable mod_rewrite
    a2enmod rewrite
## add PHP Exension
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) \
        ctype \
        dom \
        gd \
        json \
        mbstring \
        session \
        simplexml \
        xml \
        zip && \

    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /usr/src/*

#### Install Grav
COPY composer.json /var/www/html
RUN chmod 777 /var/www/ -R && \
    composer install

RUN ln -s vendor/getgrav/grav && \
    chmod 777 . -R
RUN cd grav && bin/gpm install admin devtools