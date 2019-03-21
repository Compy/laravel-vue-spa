FROM php:7.2-fpm

RUN apt-get update \
    && apt-get install -y \
        zlib1g-dev \
        libicu-dev \
        g++ \
        locales \
        curl \
        libz-dev \
        libmemcached-dev \
        libxml2-dev \
        libpng-dev \
        graphviz \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen en_US.UTF-8 \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && pecl install xdebug-2.6.0 \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-install intl \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-install gd soap mbstring xml \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    apt-get install -y --no-install-recommends git zip

RUN curl --show-error https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/app

EXPOSE 9000

# Workaround https://bugs.php.net/bug.php?id=71880
ENV LOG_STREAM="/tmp/stdout"
RUN mkfifo $LOG_STREAM && chmod 777 $LOG_STREAM

CMD ["/bin/sh", "-c", "php-fpm --pid /usr/local/var/run/php-fpm.pid | tail -f $LOG_STREAM"]