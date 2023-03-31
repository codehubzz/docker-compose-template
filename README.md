# Настройка контейнеров
### Создать .ENV для docker-compose:
```env
APP_PATH=./www
APP_NAME=promotion
APP_NETWORK=${APP_NAME}_network
TZ=Europe/Kiev
PHP_VER=8.0
#NODE_VER=14 #COMMENT THIS LINE FOR LATEST VER
PHP_FPM_CONF=xlaravel.pool.conf
```
## Настройка портов nginx (если нужно) в docker-compose.yml
```yml
nginx:
  ...
  ports:
    - "80:80"
    - "443:443"
```
# Начальная настройка самого проекта

## Клонировать проект
Создаем папку для файлов проекта и настраиваем для нее права www-data
```sh
mkdir www
chown -R 1000:1000 www
chmod -R g+s www
```
Сборка и запуск контейнеров
```sh
docker-compose build && docker-compose up -d
```
Заходим в сервис php через docker-compose:
```sh
docker-compose exec php bash
```
Клонируем проект в текущую папку
```sh
git init .
git remote add origin git@gitlab.sweet.tv:php/promotion.git
git pull origin master
```

Создаем .env для проекта и вставляем в этот файл нужные env
```sh
vim .env
```

## Установка зависимостей composer и npm
Устанавливаем из консоли контейнера зависимости composer
```sh
composer install && composer update
```
Устанавливаем из консоли контейнера зависимости npm
```sh
npm install && npm run prod
```

## Готово
Через порт который указан в docker-compose.yml должен быть доступен проект
```sh
http://localhost:88/
```

## Supervisor
Для того чтобы работал сервис supervisor нужно раскоментировать строки для этого сервиса в docker-compose.xml
```sh
#  supervisor:
#    container_name: ${APP_NAME}_supervisor
#    build: images/supervisor/php-${PHP_VER}-cli-alpine
#    ...
#
```

## Crontab
Для того чтобы работал сервис cron нужно раскоментировать строки для этого сервиса в docker-compose.xml
```sh
#  cron:
#    container_name: ${APP_NAME}_cron
#    build: ./images/cron
#    ...
#
```
##### build / start / stop
```sh
docker-compose build cron
docker-compose up -d cron
docker-compose stop cron
```
##### Logs
```sh
docker container logs cron
```
##### Зайти в контейнер
```sh
docker-compose exec cron sh
```
##### Посмотреть статус контейнеров
```sh
docker-compose ps
```


