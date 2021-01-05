FROM ubuntu:bionic

RUN ln -snf /usr/share/zoneinfo/Europe/Moscow /etc/localtime && echo Europe/Moscow > /etc/timezone

RUN apt-get update && apt-get install -y gnupg
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

COPY nginx.list /etc/apt/sources.list.d/
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62
RUN apt-get update && apt-get install -y nginx

RUN sed -i 's/user  nginx;/user  www-data;/g' /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/

CMD ["nginx", "-g", "daemon off;"]

EXPOSE 80
