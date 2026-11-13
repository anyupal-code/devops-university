# Задания к Уроку 9

## Задание 1. Подготовка секретов
1. Зарегистрируйтесь на [Docker Hub](https://hub.docker.com/) (если еще нет аккаунта).
2. В настройках вашего репозитория на GitHub (`Settings` -> `Secrets and variables` -> `Actions`) добавьте два секрета:
   * `DOCKER_USERNAME` — ваш логин на Docker Hub.
   * `DOCKER_PASSWORD` — ваш пароль (или Personal Access Token).

## Задание 2. Создание workflow для публикации
1. В папке `.github/workflows/` создайте файл `docker-publish.yml`.
2. Вставьте туда конфигурацию из теории (пример для Docker Hub).
3. Сделайте коммит и пуш.

## Задание 3. Проверка результата
1. Перейдите на вкладку **Actions** в GitHub и убедитесь, что workflow прошел успешно.
2. Зайдите на [Docker Hub](https://hub.docker.com/), откройте свой профиль и проверьте, что образ `backend-app` появился в списке репозиториев.

## Задание 4 (*). Использование GitHub Container Registry
1. В том же workflow добавьте права:
   ~~~yaml
   permissions:
     contents: read
     packages: write
   ~~~
2. Замените шаг авторизации на вход в `ghcr.io` (из теории).
3. Измените тег образа на `ghcr.io/ваш-логин/backend-app:latest`.
4. Сделайте коммит и пуш. Проверьте, что образ появился в разделе **Packages** вашего профиля GitHub.