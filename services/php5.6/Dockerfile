FROM php:5.6-fpm-alpine

LABEL maintainer="RunnerLee <runnerleer@gmail.com>"

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# 安装依赖及部分扩展
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS openssl-dev libmemcached-dev \
    && apk add freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev bzip2-dev \
    && docker-php-ext-install bcmath pdo pdo_mysql mysql mysqli pcntl sockets gd zip bz2 opcache

RUN pecl install xdebug-2.5.5 mongo memcached-2.2.0 redis-4.3.0 \
    && docker-php-ext-enable mongo memcached xdebug redis

# 安装 xhprof
RUN wget https://github.com/phacility/xhprof/archive/master.zip \
    && unzip master.zip \
    && cd xhprof-master/extension \
    && phpize \
    && ./configure --with-php-config=php-config \
    && make && make install \
    && docker-php-ext-enable xhprof \
    && unlink ../../master.zip \
    && rm -rf ../../xhprof-master

# 清理
RUN rm -rf /usr/local/src/* \
    && rm -rf /tmp/pear/download/* \
    && rm -rf /tmp/pear/cache/*

EXPOSE 9000
