# Справочник по деплою

## Расшифровка названий
* **CI/CD** — *Continuous Integration / Continuous Deployment*.
* **SSH** — *Secure Shell* (Безопасная оболочка). Протокол удаленного доступа.
* **appleboy/ssh-action** — готовое действие для выполнения команд на сервере по SSH.
* **docker-compose pull** — скачать новые версии образов.
* **docker system prune** — очистить неиспользуемые образы.

## Настройка SSH
* `ssh-keygen -t ed25519 -C "github-actions"` — создать ключ.
* `cat ~/.ssh/id_ed25519.pub` — посмотреть публичный ключ.
* `ssh-copy-id user@host` — скопировать ключ на сервер (альтернатива ручному копированию).

## Основные команды для деплоя на сервере
* `cd /path/to/project` — перейти в папку проекта.
* `docker-compose pull` — скачать обновления.
* `docker-compose up -d` — перезапустить контейнеры.
* `docker system prune -f` — очистить старые образы.