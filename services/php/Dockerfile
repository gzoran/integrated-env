ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm

RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list \
	&& rm -rf /var/lib/apt/lists/* \
	&& cat /etc/apt/sources.list \
	&& apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        zip \
        unzip \
        libzip-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install bcmath \
        && docker-php-ext-install pcntl \
        && docker-php-ext-install pdo_mysql \
        && curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
	&& chmod +x /usr/bin/composer \
        && usermod -u 1000 www-data && groupmod -g 1000 www-data
	    
ENV COMPOSER_HOME=/tmp/composer

WORKDIR /var/www

