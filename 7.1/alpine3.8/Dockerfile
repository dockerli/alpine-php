FROM php:7.1.23-fpm-alpine3.8

RUN echo "http://mirrors.ustc.edu.cn/alpine/v3.7/main" > /etc/apk/repositories && echo "http://mirrors.ustc.edu.cn/alpine/v3.7/community" >> /etc/apk/repositories

RUN set -xe \
    && apk add --no-cache --virtual .build-deps \
    $PHPIZE_DEPS \
    coreutils \
    curl-dev \
    libedit-dev \
    libressl-dev \
    libxml2-dev \
    sqlite-dev \
    freetype-dev \
    libjpeg-turbo-dev \ 
    libxpm-dev \
    gettext-dev \
    icu-dev \
    libmcrypt-dev \
    libxslt-dev \
    libwebp-dev \
    rabbitmq-c-dev \
    linux-headers \
    \
    && docker-php-source extract \
    && wget -O /usr/src/swoole.tar.gz https://github.com/swoole/swoole-src/archive/v4.2.2.tar.gz \
    && mkdir /usr/src/php/ext/swoole && tar -zxf /usr/src/swoole.tar.gz -C /usr/src/php/ext/swoole --strip-components=1 \ 
    \
    && wget -O /usr/src/redis.tar.gz https://github.com/phpredis/phpredis/archive/4.1.1.tar.gz \
    && mkdir /usr/src/php/ext/redis && tar -zxf /usr/src/redis.tar.gz -C /usr/src/php/ext/redis --strip-components=1 \
    \
    && wget -O /usr/src/amqp.tar.gz https://github.com/pdezwart/php-amqp/archive/v1.9.3.tar.gz \
    && mkdir /usr/src/php/ext/amqp && tar -zxf /usr/src/amqp.tar.gz -C /usr/src/php/ext/amqp --strip-components=1 \
    && docker-php-ext-install amqp redis swoole opcache bcmath sockets exif zip gd gettext intl mcrypt pcntl pdo_mysql shmop sysvsem xmlrpc xsl \
    && cd / \
    && docker-php-source delete \
    \
    && runDeps="$( \
        scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
            | tr ',' '\n' \
            | sort -u \
            | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )" \
    && apk add --no-cache --virtual .php-rundeps $runDeps \
    \
    && apk del .build-deps \
    \
    && rm -f /usr/local/lib/php/extensions/no-debug-non-zts-20160303/*.a

CMD ["php-fpm"]