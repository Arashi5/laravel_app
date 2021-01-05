FROM ubuntu:bionic

RUN ln -snf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && echo Europe/Moscow > /etc/timezone

RUN apt-get update && apt-get install -y software-properties-common
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

RUN add-apt-repository -y ppa:ondrej/php && apt-get update

RUN apt-get install -y php7.4-fpm php7.4-bcmath php7.4-json php7.4-mbstring php7.4-zip php7.4-curl php7.4-gd php7.4-opcache php7.4-xml php7.4-pgsql
RUN apt-get update && apt-get install -y php-xdebug

COPY www.conf /etc/php/7.4/fpm/pool.d/

RUN mkdir /run/php

COPY xdebug.ini /etc/php/7.4/mods-available/xdebug.ini
COPY php.ini  /etc/php/7.4/fpm/php.ini

CMD ["php-fpm7.4", "-F"]

EXPOSE 9000
