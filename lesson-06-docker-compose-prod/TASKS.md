# Задания к Уроку 6

## Задание 1. Настройка сети и ограничений
1. Откройте ваш `docker-compose.yml` из проекта Spring Boot.
2. Добавьте ограничения ресурсов для сервиса `app`:
   ~~~yaml
   deploy:
     resources:
       limits:
         cpus: '1.0'
         memory: 512M
   ~~~
3. Запустите `docker-compose up`.
4. Убедитесь, что приложение работает (откройте в браузере `http://localhost:8080`).

## Задание 2. Политика перезапуска
1. Добавьте в `docker-compose.yml` для сервиса `app` и `db` политику перезапуска:
   ~~~yaml
   restart: always
   ~~~
2. Запустите `docker-compose up -d`.
3. Найдите ID контейнера приложения: `docker ps`.
4. Убейте процесс внутри контейнера (или перезапустите его): `docker restart <container_id>`.
5. Убедитесь, что контейнер автоматически поднялся (посмотрите `docker ps`).

## Задание 3 (*). Использование секретов
1. Создайте файл `db_password.txt` с паролем:
   ~~~text
   mypassword
   ~~~
2. В `docker-compose.yml` для сервиса `db` добавьте:
   ~~~yaml
   secrets:
     - db_password
   environment:
     POSTGRES_PASSWORD_FILE: /run/secrets/db_password
   ~~~
   И в конец файла:
   ~~~yaml
   secrets:
     db_password:
       file: ./db_password.txt
   ~~~
3. Перезапустите: `docker-compose down && docker-compose up -d`.
4. Проверьте, что база данных запустилась (логи не должны содержать ошибок).

**Важно:** убедитесь, что файл `db_password.txt` добавлен в `.gitignore`, чтобы он не попал в репозиторий.