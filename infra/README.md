# Локальная база данных

Требуются запущенный Docker Desktop и Docker Compose.
Все команды ниже выполняются из папки infra.

## Первоначальная настройка

Один раз создай локальный пароль:

```bash
printf 'POSTGRES_PASSWORD=%s\n' "$(openssl rand -hex 24)" > .env
```

Файл .env не добавляется в Git.
Не перезаписывай его после создания базы: изменение файла
само по себе не меняет пароль существующей PostgreSQL.

## Запуск

```bash
docker compose up -d
docker compose ps
```

После запуска ожидается статус healthy.

## Проверка подключения

```bash
docker compose exec db psql -P pager=off -U pomodoro -d pomodoro_focus -c "SELECT current_database(), version();"
```

## Параметры подключения с компьютера

- Адрес: localhost
- Порт: 5433
- База: pomodoro_focus
- Пользователь: pomodoro
- Пароль: значение POSTGRES_PASSWORD из локального .env

Для будущих сервисов в той же сети Compose:
адрес db, порт 5432.

## Остановка

```bash
docker compose stop
```

Повторный запуск:

```bash
docker compose up -d
```

Данные сохраняются в Docker volume между остановками.
Команда docker compose down -v удаляет этот volume
и данные базы — для обычной остановки она не нужна.

## Текущее состояние

Подготовлена пустая PostgreSQL для локальной разработки.
Таблицы приложения и загрузчик демонстрационных данных
пока не реализованы.
