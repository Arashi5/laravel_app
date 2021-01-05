FROM ubuntu:bionic

RUN ln -snf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && echo Europe/Moscow > /etc/timezone

RUN apt-get update && apt-get install -y software-properties-common wget curl sudo nano mc bash-completion libjemalloc1 libjemalloc-dev gcc make
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
RUN mkdir /var/www && chown -R www-data:www-data /var/www
COPY .bashrc /var/www
RUN chown -R www-data:www-data /var/www

RUN add-apt-repository -y ppa:ondrej/php && apt-get update

RUN apt-get install -y php7.4-cli php7.4-bcmath php7.4-json php7.4-mbstring php7.4-zip php7.4-curl php7.4-gd php7.4-opcache php7.4-xml php7.4-pgsql

# Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

# NPM
USER www-data
RUN cd /var/www && \
    wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" && \
    nvm install 10 && \
    nvm use 10

# Postgres client
USER root
COPY pgdg.list /etc/apt/sources.list.d/
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN apt-get update && apt-get install -y postgresql-client-11

# Crontab
COPY crontab /var/www/crontab
RUN crontab -u www-data /var/www/crontab && rm -f /var/www/crontab

# Redis client
RUN cd /tmp && \
    wget http://download.redis.io/redis-stable.tar.gz && \
    tar xvzf redis-stable.tar.gz && \
    cd redis-stable && \
    make && \
    cp src/redis-cli /usr/local/bin/ && \
    chmod 755 /usr/local/bin/redis-cli

RUN apt-get install libpng-dev -y --no-install-recommends

USER www-data
WORKDIR /var/www/app
