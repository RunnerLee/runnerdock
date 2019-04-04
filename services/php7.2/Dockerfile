FROM php:7.2-fpm-alpine

LABEL maintainer="RunnerLee <runnerleer@gmail.com>"

# 安装扩展及依赖
RUN apk add --no-cache --update libmemcached-libs zlib bzip2-dev openssl openssl-dev \
    freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && set -xe \
    && docker-php-ext-install bcmath pdo_mysql mysqli pcntl sockets gd zip bz2 \
    && pecl install -o -f redis \
    && pecl download swoole-4.3.1 \
    && tar zvxf swoole-4.3.1.tgz \
    && cd swoole-4.3.1 \
    && phpize \
    && ./configure --with-php-config=php-config --enable-sockets --enable-openssl \
        --enable-http2 --enable-mysqlnd --with-openssl-dir=/usr/lib \
    && make \
    && make install \
    && cd ../ \
    && unlink swoole-4.3.1.tgz \
    && rm -rf swoole-4.3.1 \
    && pecl install xdebug \
    && docker-php-ext-enable redis swoole xdebug \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && unlink composer-setup.php \
    && mv composer.phar /usr/local/bin/composer \
    && rm -rf /usr/local/src/* \
    && rm -rf /tmp/*

EXPOSE 9000