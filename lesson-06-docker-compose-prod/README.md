# Урок 6. Docker Compose в продакшне

## Цель урока
Научиться настраивать сети между контейнерами, ограничивать ресурсы (CPU и память) и безопасно управлять секретами с помощью Docker Compose.

## Что потребуется для этого урока
* **Сервер или локальная машина с Docker и Docker Compose**.
* **Файл `docker-compose.yml`** из прошлых уроков (мы его модифицируем).

---

## Теория

### 1. Сети в Docker Compose
По умолчанию Docker Compose создает одну общую сеть для всех сервисов. Контейнеры могут обращаться друг к другу по имени сервиса (например, `db` или `app`), а не по IP-адресу.
Это позволяет легко связывать базу данных и приложение:

~~~yaml
services:
  app:
    build: .
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydb
~~~

* `depends_on` — гарантирует, что `db` запустится раньше `app`.
* В `SPRING_DATASOURCE_URL` мы используем `db` вместо `localhost` — это имя сервиса (контейнера).

### 2. Ограничение ресурсов (CPU и память)
В продакшне нельзя позволять одному контейнеру съесть все ресурсы сервера. Docker Compose позволяет задать лимиты:

~~~yaml
services:
  app:
    build: .
    deploy:
      resources:
        limits:
          cpus: '1.0'      # Максимум 1 ядро CPU
          memory: 512M     # Максимум 512 МБ ОЗУ
        reservations:
          cpus: '0.5'      # Гарантированно 0.5 ядра
          memory: 256M     # Гарантированно 256 МБ ОЗУ
~~~

* `limits` — жесткий потолок. Если приложение попытается использовать больше, оно будет остановлено (OOM killer).
* `reservations` — минимальная гарантия. Docker зарезервирует эти ресурсы.

### 3. Секреты (secrets)
Хранить пароли в переменных окружения (`environment`) небезопасно: они могут попасть в логи или в историю команд.
Docker Compose позволяет передавать секреты через файлы:

~~~yaml
services:
  db:
    image: postgres:16-alpine
    secrets:
      - db_password
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password

secrets:
  db_password:
    file: ./db_password.txt
~~~

* `secrets` — файл с паролем (например, `db_password.txt`), который не попадает в образ.
* `POSTGRES_PASSWORD_FILE` — указывает Postgres прочитать пароль из файла внутри контейнера.

### 4. Политики перезапуска (restart)
Если контейнер упал, Docker может перезапустить его автоматически. Это критически важно для продакшна.

~~~yaml
services:
  app:
    restart: always
~~~

Варианты:
* `no` — не перезапускать (по умолчанию).
* `always` — перезапускать всегда.
* `on-failure` — перезапускать только при ошибке.
* `unless-stopped` — перезапускать, если не был остановлен вручную.

### 5. Переменные окружения и файлы .env
Вы уже знаете, что переменные окружения можно передавать через `environment`.
Для удобства в `docker-compose.yml` можно использовать переменные из файла `.env`:

~~~yaml
environment:
  DB_PASSWORD: ${DB_PASSWORD}
~~~

Файл `.env` в корне проекта:
~~~env
DB_PASSWORD=supersecret
~~~

---

## Практика
Переходите к [Справочнику команд](COMMANDS.md) и [Заданиям](TASKS.md).

---

## Чек-лист
- [ ] Понимаю, как работает сеть в Docker Compose.
- [ ] Умею ограничивать ресурсы (`limits` и `reservations`).
- [ ] Понимаю, зачем нужны секреты (`secrets`).
- [ ] Умею настраивать `restart` политики.
- [ ] Умею использовать `.env` для переменных.

---

## Что дальше
На следующем уроке разберем **CI/CD**: что это такое и как автоматизировать сборку.