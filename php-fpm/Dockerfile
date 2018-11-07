FROM phpdockerio/php72-fpm:latest
WORKDIR "/app"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# 阿里云源
COPY ./sources.list /etc/apt/sources.list

# 安装PHP扩展
RUN apt-get update \
    && apt-get -y --no-install-recommends install  php7.2-mysql php-redis php-xdebug php7.2-gd php-imagick \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# 修改 composer 为国内镜像
RUN composer config -g repo.packagist composer https://packagist.laravel-china.org