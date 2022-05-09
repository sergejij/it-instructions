# PostgreSQL

## Установка в качестве Docker-контейнера

Базовая команда для установки контейнера выглядит так:
```
docker run --name localhost-postgres -p 5432:5432 -e POSTGRES_USER=some_user -e POSTGRES_PASSWORD=some_password -e POSTGRES_DB=default_db_name -d postgres
```
- ```--name``` - аргумент параметра задает название контейнеру

Здесь указано несколько переменных окружения (флаги `-e`):
- POSTGRES_USER — имя суперпользователя
- POSTGRES_PASSWORD — пароль суперпользователя
- POSTGRES_DB — название базы данных по умолчанию

За дополнительной информацией можно обратиться [сюда](https://hub.docker.com/_/postgres).

## Подключение к Postgres через psql

Команда для подключения:
```
psql -U user -d database
```
Флаг -U устанавливает пользователя, под именем подключаемся.
Флаг -d определяет базу данных, к которой подключаемся.

## Полезные ссылки

> В случае, если параметр содержит в себе символы '-' (например имя пользователя: `my-user`), его нужно оборачивать в двойные кавычки: `create user "my-user"`

- Создать базу; создать пользователя; дать пользователю права на работу с базой: [тык](https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e)
- [Создать таблицу](https://www.tutorialspoint.com/postgresql/postgresql_create_table.htm)
