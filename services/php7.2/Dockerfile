FROM php:7.2-fpm-alpine

LABEL maintainer="RunnerLee <runnerleer@gmail.com>"

# 安装依赖及部分扩展
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && docker-php-ext-install bcmath pdo_mysql mysqli pcntl sockets

# 安装 redis 扩展
RUN pecl install -o -f redis \
    && docker-php-ext-enable redis

# 安装 gd 扩展
RUN set -xe \
    && apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-install gd

# 安装 zip bz2 扩展
RUN set -xe \
    && apk add --no-cache bzip2-dev \
    && docker-php-ext-install zip bz2

# 安装 swoole 扩展
RUN set -xe \
    && apk add --no-cache openssl openssl-dev \
    && pecl download swoole-4.3.1 \
    && tar zvxf swoole-4.3.1.tgz \
    && cd swoole-4.3.1 \
    && phpize \
    && ./configure --with-php-config=php-config --enable-sockets --enable-openssl \
        --enable-http2 --enable-mysqlnd --with-openssl-dir=/usr/lib \
    && make \
    && make install \
    && docker-php-ext-enable swoole \
    && unlink ../swoole-4.3.1.tgz \
    && rm -rf ../swoole-4.3.1

# 安装 xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# 安装 mongodb
RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

# 安装 memcached 扩展
RUN set -xe \
    && apk add --no-cache libmemcached-dev \
    && pecl install memcached \
    && docker-php-ext-enable memcached

# 安装 protobuf 扩展
RUN set -xe \
    && pecl install protobuf \
    && docker-php-ext-enable protobuf

# 安装 composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && unlink composer-setup.php \
    && mv composer.phar /usr/local/bin/composer

# 清理
RUN rm -rf /usr/local/src/* \
    && rm -rf /tmp/pear/download/* \
    && rm -rf /tmp/pear/cache/*

EXPOSE 9000