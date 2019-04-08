FROM php:7.3.3-fpm-alpine3.9
RUN mkdir -p /var/wwwroot
ENV TIMEZONE Asia/Shanghai
ENV PHPREDIS_VERSION 4.3.0
#ENV PHPSWOOLE_VERSION 4.3.1
ENV PHPXDEBUG 2.7.1

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
&& apk update \
&& apk add tzdata git ansible && ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone \
&& mkdir -p /usr/src/php/ext \

#添加REDIS
&& curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
&& tar xfz /tmp/redis.tar.gz \
&& rm -r /tmp/redis.tar.gz \
&& mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
&& docker-php-ext-install redis \
# #添加SWOOOLE
# && curl -L -o /tmp/swoole.tar.gz https://github.com/swoole/swoole-src/archive/v$PHPSWOOLE_VERSION.tar.gz \
# && tar xfz /tmp/swoole.tar.gz \
# && rm -r /tmp/swoole.tar.gz \
# && mv swoole-src-$PHPSWOOLE_VERSION /usr/src/php/ext/swoole \
# && docker-php-ext-install swoole \
#添加xdebug
&& curl -L -o /tmp/xdebug.tar.gz https://github.com/xdebug/xdebug/archive/$PHPXDEBUG.tar.gz \
&& tar xfz /tmp/xdebug.tar.gz \
&& rm -r /tmp/xdebug.tar.gz \
&& mv xdebug-$PHPXDEBUG /usr/src/php/ext/xdebug \
&& docker-php-ext-install xdebug \
#添加自有主键
&& docker-php-ext-install mbstring opcache pdo pdo_mysql \
#添加扩展 gd zip
&& apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev zlib-dev libzip-dev libjpeg-turbo-dev && \
    docker-php-ext-configure gd \
      --with-gd \
      --with-freetype-dir=/usr/include/ \
      --with-png-dir=/usr/include/ \
      --with-jpeg-dir=/usr/include/ \
      --with-zlib-dir=/usr && \
        NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
        docker-php-ext-install -j${NPROC} gd zip && \
        apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev \
&& rm -rf /usr/src/php
COPY php.ini /usr/local/etc/php/
WORKDIR /var/wwwroot
user root
