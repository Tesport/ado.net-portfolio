# AutoPartsApp

Учебный проект по ADO.NET: веб-приложение «Автозапчасти» на ASP.NET Core MVC (.NET 8) с базой PostgreSQL, запускаемое в Docker.

## Что сделано

- **DbHelper** (`Data/DbHelper.cs`): класс доступа к данным на чистом ADO.NET (Npgsql: `NpgsqlConnection`, `NpgsqlCommand`, `NpgsqlDataReader`). Реализованы:
  - `Initialize()`: создаёт таблицу `parts` и заполняет тестовыми данными, если она пуста;
  - `GetAllParts()` и `GetPartById()`: чтение списка и одной записи;
  - `AddPart()`: добавление записи;
  - `DecreaseStock()`: покупка, атомарный `UPDATE ... WHERE stock >= @qty`.
- Все запросы параметризованы (`@id`, `@name` и т. д.), поэтому SQL-инъекции исключены.
- **Docker**: `docker-compose.yml` поднимает два контейнера, `db` (PostgreSQL 16) и `web` (приложение). У базы настроен healthcheck, `web` стартует после неё. Данные хранятся в томе `pgdata` и сохраняются между перезапусками.
- `Dockerfile` собирает приложение в два этапа (SDK для сборки, ASP.NET runtime для запуска).
- Строка подключения передаётся в контейнер переменной окружения `ConnectionStrings__Default`.

## Запуск

```bash
docker compose up -d --build
```

Сайт: http://localhost:8080. Остановка: `docker compose down`.
