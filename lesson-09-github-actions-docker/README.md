# Урок 9. GitHub Actions: Публикация Docker-образа

## Цель урока
Научиться автоматически собирать Docker-образ в GitHub Actions и отправлять его в реестр (Docker Hub или GitHub Container Registry).

## Что потребуется для этого урока
* **Репозиторий на GitHub** с Java-проектом и `Dockerfile`.
* **Аккаунт на Docker Hub** (или использование GitHub Container Registry).

---

## Теория

### 1. Зачем публиковать образ в CI/CD?
В прошлом уроке мы собирали `.jar` файл. Но для деплоя на сервер удобнее использовать готовый Docker-образ. 
Если образ собирается автоматически при каждом пуше, мы всегда имеем актуальную версию, готовую к запуску на любом сервере одной командой `docker pull`.

### 2. Выбор реестра
* **Docker Hub** — самый популярный публичный реестр.
* **GitHub Container Registry (ghcr.io)** — встроен в GitHub, не требует отдельной регистрации (использует ваш GitHub-аккаунт).

### 3. Аутентификация в реестре
Чтобы отправить образ, нужно авторизоваться.
* **Для Docker Hub:** логин и пароль (или Personal Access Token). Их нужно сохранить в настройках репозитория GitHub: `Settings -> Secrets and variables -> Actions`. Создайте секреты `DOCKER_USERNAME` и `DOCKER_PASSWORD`.
* **Для GitHub Container Registry:** используется встроенный токен `GITHUB_TOKEN`, который доступен автоматически. Достаточно добавить разрешение `packages: write` в workflow.

### 4. Пример workflow для Docker Hub
~~~yaml
name: Build and Push Docker Image

on:
  push:
    branches: [ "main" ]

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up JDK 21
      uses: actions/setup-java@v4
      with:
        java-version: '21'
        distribution: 'temurin'

    - name: Build with Maven
      run: mvn -B clean package -DskipTests

    - name: Log in to Docker Hub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ secrets.DOCKER_USERNAME }}/backend-app:latest
~~~

### 5. Разбор ключевых шагов
* **`docker/login-action@v3`** — авторизация в Docker Hub с использованием секретов.
* **`docker/build-push-action@v5`** — сборка и отправка образа. Параметр `tags` задает имя и тег образа (например, `username/backend-app:latest`).

### 6. Использование GitHub Container Registry (альтернатива)
Если не хотите использовать Docker Hub, можно отправлять образ в `ghcr.io`:
~~~yaml
    - name: Log in to GitHub Container Registry
      uses: docker/login-action@v3
      with:
        registry: ghcr.io
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - name: Build and push
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ghcr.io/${{ github.repository }}/backend-app:latest
~~~
Для этого в workflow нужно добавить права:
~~~yaml
permissions:
  contents: read
  packages: write
~~~

---

## Практика
Переходите к [Справочнику команд](COMMANDS.md) и [Заданиям](TASKS.md).

---

## Чек-лист
- [ ] Понимаю, зачем публиковать Docker-образ в CI/CD.
- [ ] Знаю, как настроить секреты для Docker Hub.
- [ ] Умею писать workflow для сборки и отправки образа.
- [ ] Понимаю, как использовать GitHub Container Registry.

---

## Что дальше
На следующем уроке разберем **Мониторинг (Prometheus и Grafana)**.