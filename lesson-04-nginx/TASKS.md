# Задания к Уроку 4

## Задание 1. Установка и проверка Nginx
1. Подключитесь к вашему серверу (или виртуальной машине) по SSH.
2. Установите Nginx: `sudo apt update && sudo apt install nginx`.
3. Откройте браузер и введите IP-адрес вашего сервера. Вы должны увидеть страницу «Welcome to nginx!».

## Задание 2. Настройка обратного прокси
1. Запустите любое Java-приложение (например, из курса по Spring Boot) на порту `8080` (или создайте простой тестовый HTTP-сервер на Python: `python3 -m http.server 8080`).
2. Создайте конфигурацию для Nginx:
   ~~~bash
   sudo nano /etc/nginx/sites-available/my-app
   ~~~
3. Вставьте туда (замените `your_ip` на IP вашего сервера):
   ~~~nginx
   server {
       listen 80;
       server_name your_ip;

       location / {
           proxy_pass http://localhost:8080;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }
   ~~~
4. Активируйте конфигурацию и удалите дефолтную:
   ~~~bash
   sudo ln -s /etc/nginx/sites-available/my-app /etc/nginx/sites-enabled/
   sudo rm /etc/nginx/sites-enabled/default
   ~~~
5. Проверьте конфигурацию и перезапустите Nginx:
   ~~~bash
   sudo nginx -t
   sudo systemctl restart nginx
   ~~~
6. Снова откройте браузер и введите IP-адрес сервера. Теперь вы должны увидеть ответ вашего Java-приложения (или Python-сервера), а не страницу Nginx.

## Задание 3 (*). HTTPS (если есть домен)
Если у вас есть домен, привязанный к IP сервера:
1. Установите Certbot: `sudo apt install certbot python3-certbot-nginx`.
2. Получите сертификат: `sudo certbot --nginx -d my-site.com` (замените на ваш домен).
3. Проверьте, что сайт открывается по `https://`.