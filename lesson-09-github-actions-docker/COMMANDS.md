# Справочник по публикации Docker-образов

## Расшифровка названий
* **Registry** — *Registry* (Реестр). Хранилище Docker-образов.
* **Docker Hub** — публичный реестр Docker.
* **ghcr.io** — GitHub Container Registry (реестр GitHub).
* **Tag** — *Tag* (Метка). Версия образа (например, `latest`, `v1.0`).
* **login-action** — действие для авторизации в реестре.
* **build-push-action** — действие для сборки и отправки образа.

## Основные команды Docker (локально)
* `docker login` — авторизация в Docker Hub.
* `docker tag my-app username/my-app:latest` — присвоить тег образу.
* `docker push username/my-app:latest` — отправить образ в реестр.
* `docker pull username/my-app:latest` — скачать образ из реестра.

## Настройка секретов в GitHub
1. Перейдите в репозиторий на GitHub.
2. Откройте `Settings` -> `Secrets and variables` -> `Actions`.
3. Нажмите `New repository secret`.
4. Добавьте `DOCKER_USERNAME` и `DOCKER_PASSWORD`.