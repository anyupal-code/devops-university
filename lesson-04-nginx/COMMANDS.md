# Справочник по Nginx

## Расшифровка названий
* **Nginx** — название не расшифровывается, это бренд (читается «Энджинкс»).
* **proxy_pass** — *Proxy Pass* (Прокси-передача). Передает запрос дальше.
* **listen** — *Listen* (Слушать). Указывает порт, который слушает сервер.
* **server_name** — *Server Name* (Имя сервера). Указывает домен или IP, для которого применяется конфигурация.
* **location** — *Location* (Расположение). Определяет, для каких URL-путей применяется правило.
* **tail** — *Tail* (Хвост). Показывает последние строки файла (полезно для чтения логов).

## Установка и управление
* `sudo apt install nginx` — установить Nginx.
* `sudo systemctl start nginx` — запустить.
* `sudo systemctl restart nginx` — перезапустить.
* `sudo systemctl status nginx` — проверить статус (работает или упал).
* `sudo nginx -t` — проверить конфигурацию на ошибки.

## Работа с конфигурацией
* `sudo nano /etc/nginx/sites-available/my-site` — создать конфигурацию для нового сайта.
* `sudo ln -s /etc/nginx/sites-available/my-site /etc/nginx/sites-enabled/` — активировать конфигурацию (создать символическую ссылку).
* `sudo rm /etc/nginx/sites-enabled/default` — удалить стандартную страницу «Welcome to nginx».

## Логи
* `sudo tail -f /var/log/nginx/access.log` — смотреть входящие запросы в реальном времени. *Флаг `-f` (follow) означает «следить»: файл не закроется, а будет обновляться по мере поступления новых записей.*
* `sudo tail -f /var/log/nginx/error.log` — смотреть ошибки.

## HTTPS
* `sudo apt install certbot python3-certbot-nginx` — установить Certbot.
* `sudo certbot --nginx -d my-site.com` — получить сертификат и настроить HTTPS.