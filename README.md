# Настройка контейнеров (на примере tv_server)
### .ENV для docker-compose:
```env
APP_PATH=./www
APP_NAME=tv_server
APP_NETWORK=${APP_NAME}_network
TZ=Europe/Kiev
```
## Настройка портов nginx (если нужно) в docker-compose.yml
```yml
nginx:
    ...
    ports:
      - "88:80"
      - "448:443"
```
# Начальная настройка самого проекта (на примере tv_server)

## Клонировать проект
Создаем папку для проекта (если не сделать - будет создана папка с правами root, а не www-data)
```sh
mkdir www
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
git remote add origin https://gitlab.sweet.tv/php/tv_server_admin.git
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


