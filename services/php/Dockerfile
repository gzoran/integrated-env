ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm
WORKDIR /var/www

# libraries
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
curl \
wget \
git \
libz-dev \
libssl-dev \
libnghttp2-dev \
libpcre3-dev \
cmake

# user
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

# composer
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
&& chmod +x /usr/bin/composer
ENV COMPOSER_HOME=/tmp/composer

# gd
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
&& docker-php-ext-install -j$(nproc) gd

# zip
RUN docker-php-ext-install zip

# bcmatn
RUN docker-php-ext-install bcmath

# pcntl
RUN docker-php-ext-install pcntl

# pdo_mysql
RUN docker-php-ext-install pdo_mysql

# redis
RUN mkdir -p /usr/src/php/ext/redis && curl -L https://github.com/phpredis/phpredis/archive/5.2.2.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
&& echo 'redis' >> /usr/src/php-available-exts && docker-php-ext-install redis

# swoole
RUN wget https://github.com/swoole/swoole-src/archive/v4.5.2.tar.gz -O swoole.tar.gz \
&& mkdir -p swoole \
&& tar -xf swoole.tar.gz -C swoole --strip-components=1 \
&& rm swoole.tar.gz \
&& ( \
cd swoole \
&& phpize \
&& ./configure --enable-openssl --enable-http2 \
&& make -j$(nproc) \
&& make install \
) \
&& rm -r swoole \
&& docker-php-ext-enable swoole

# amqp
RUN apt-get install -y librabbitmq-dev \
&& echo '\n' | pecl install amqp && docker-php-ext-enable amqp

# xlswriter
RUN echo 'yes' | pecl install xlswriter && docker-php-ext-enable xlswriter

