FROM php:5.6-fpm-alpine

LABEL maintainer="RunnerLee <runnerleer@gmail.com>"

# 安装依赖及部分扩展
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && docker-php-ext-install bcmath pdo pdo_mysql mysql mysqli pcntl

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

# 安装 xdebug
RUN pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug

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

# 安装 mongodb
RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

# 安装 memcached 扩展
RUN set -xe \
    && apk add --no-cache libmemcached-dev \
    && pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached

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
