## Стек технологий

* Nginx
* PHP 8
* PostgreSQL
* Redis

## Работа с Docker контейнером

При первоначальном запуске контейнера необходимо:

1. Создать файл .env по образцу .env.example в корне проекта, заполнить параметры подключения к БД.
2. Выполнить сборку контейнеров: `docker-compose build`.
3. Запустить контейнеры: `docker-compose up -d`.
4. Войти в контейнер workspace `docker exec -t -i app_workspace_1 /bin/bash`
5. В контейнере:
    * Выполнить команду `composer install` для установки PHP зависимостей.
    * Выполнить `php artisan key:generate` для генерации ключа приложения.
    * Выполнить `php artisan storage:link` для создания симлинка хранилища.

После выполнения первоначальной настройки контейнер для разработки запускается командой `docker-compose up -d`.

Остановка контейнера осуществляется командой `docker-compose stop`. Удаление инстансов контейнеров: `docker-compose down`

После запуска контейнеров сайт будет доступен по адресу: http://localhost/.

Вся работа внутри Docker осуществляется на контейнере `workspace`. Вход в контейнер осуществляется командой:
```
docker exec -t -i app_workspace_1 /bin/bash
```

Внутри контейнера доступны следующие команды:

* `php` - консольный интерпретатор PHP
* `psql --host=postgres --port=5432 --username=USER_NAME --dbname=DB_NAME= --password ` - менеджер зависимостей PHP
* `redis-cli` - консольный клиент Redis
* `npm` - менеджер зависимостей JavaScript

## Запуск автотестов

1. Запустить тестовые контейнеры `docker-compose --f docker-compose.testing.yml up -d --b`
2. Зайти в контейнер `docker exec -it app_php-fpm-test_1 bash`
3. Выполнить команду `php /var/www/app/vendor/phpunit/phpunit/phpunit --configuration /var/www/app/phpunit.xml /var/www/app/tests`
