## Django-app workflow

[![Django-app workflow](https://github.com/brainteaser-ov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/brainteaser-ov/yamdb_final/actions/workflows/yamdb_workflow.yml)

# Yamdb API

## REST API для сервиса YaMDb — базы отзывов о фильмах, книгах и музыке


### Описание
***

__Отзывы__

Получить список всех отзывов, создать новый отзыв, получить отзыв по id, частично обновить отзыв по id, удалить отзыв по id
***
__Коментарии к отзывам__

Получить список всех комментариев к отзыву по id, создать новый комментарий для отзыва, получить комментарий для отзыва по id, частично обновить комментарий к отзыву по id, удалить комментарий к отзыву по id
***
__Пользователи__

Получить список всех пользователей, создание пользователя, получить пользователя по username, изменить данные пользователя по username, удалить пользователя по username, получить данные своей учетной записи, изменить данные своей учетной записи
***
__Категории произведений__

Получить список всех категорий, создать категорию, удалить категорию
***
__Категории жанров__

Получить список всех жанров, создать жанр, удалить жанр
***

__Произведения, к которым пишут отзывы__

Получить список всех объектов, создать произведение для отзывов, информация об объекте, обновить информацию об объекте, удалить произведение
***


## Workflow:

* Тестирование проекта (pytest, flake8).
* Сборка и публикация образа на DockerHub.
* Автоматический деплой на удаленный сервер.
* Отправка уведомления в телеграм-чат.

## Как запустить проект

### 1. Клонировать репозиторий и перейти в него в командной строке

```
git clone git@github.com:brainteaser-ov/yamdb_final.git
```
#### В Settings - Secrets - Actions добавить переменные

```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
SECRET_KEY='Очень секретно'
```
> **SECRET_KEY** используется для криптографической подписи (для генерации хэшей и токенов), длина ключа - 50 символов
Генератор ключей **Django**:
```
https://djecrety.ir
DOCKER_PASSWORD=<Docker password>
DOCKER_USERNAME=<Docker username>
USER=<username для подключения к серверу>
HOST=<IP сервера>
PASSPHRASE=<пароль для сервера, если он установлен>
SSH_KEY=<ваш SSH ключ(cat ~/.ssh/id_rsa)>
TG_CHAT_ID=<ID чата, в который придет сообщение>
TELEGRAM_TOKEN=<токен вашего бота>
```
#### Выполните вход на удаленный сервер

```
 ssh <username>@<ip>
```

#### Установите docker на сервер

```
https://docs.docker.com/get-docker/
```

#### Установите docker-compose на сервер(актуальная версия [тут](https://github.com/docker/compose/releases))

```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
#### Запустите docker-compose:
```
docker-compose up -d --build
```
#### Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер

```
scp docker-compose.yaml <username>@<host>:/home/<username>/docker-compose.yaml
```

```
scp -r nginx <username>@<host>:/home/<username>/
```

#### Собрать статические файлы в STATIC_ROOT

```
docker-compose exec web python3 manage.py collectstatic --noinput
```

#### Применить миграции

```
docker-compose exec web python3 manage.py migrate --noinput
```

#### Заполнить базу данных

```
docker-compose exec web python3 manage.py loaddata fixtures.json
```

#### Создать суперпользователя Django

```
docker-compose exec web python manage.py createsuperuser
```

#### Проект будет доступен по адресу

```
http://51.250.78.172/admin

http://51.250.78.172/api/v1/categories/
http://51.250.78.172/api/v1/users/
http://51.250.78.172/api/v1/titles/
```

#### Документация API

```
http://51.250.78.172/redoc/
```

#### [Образ на DockerHub](https://hub.docker.com/repository/docker/brainteaser/api_yamdb)

#### Стек
![](https://img.shields.io/badge/Python%20-3-informational) ![](https://img.shields.io/badge/django-project-yellow) ![](https://img.shields.io/badge/django-DRF-ff69b4) ![](https://img.shields.io/badge/Docker-Container-success) ![](https://img.shields.io/badge/Postgre-SQL-blueviolet) ![](https://img.shields.io/badge/nginx-org-ff69b4) ![](https://img.shields.io/badge/gunicorn-org-green)
![](https://img.shields.io/badge/simple-JWT-red)
- Веб-сервер: [nginx](https://nginx.org/ru/) (контейнер nginx)
- Backend фреймворк: [Django](https://www.djangoproject.com) (контейнер web)
- API фреймворк: [Django REST](https://www.django-rest-framework.org) (контейнер web)
- База данных: [PostgreSQL](https://www.postgresql.org) (контейнер db)

#### Directed by [Оксана Гончарова](https://github.com/brainteaser-ov)

#### Happy New Year!