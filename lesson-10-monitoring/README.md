# Урок 10. Мониторинг (Prometheus и Grafana)

## Цель урока
Научиться разворачивать стек мониторинга (Prometheus и Grafana), собирать системные метрики с сервера и визуализировать их на дашбордах.

## Что потребуется для этого урока
* **Сервер или локальная машина с Docker и Docker Compose**.
* **Базовые знания YAML** (из уроков 6 и 8).

---

## Теория

### 1. Зачем нужен мониторинг?
В DevOps недостаточно просто запустить приложение. Нужно знать, что с ним происходит:
* Сколько памяти и CPU оно потребляет.
* Сколько запросов в секунду обрабатывает.
* Не упало ли оно ночью.
Без мониторинга вы узнаете о проблеме только от пользователей. С мониторингом — увидите график, который пошел вверх, и предотвратите сбой заранее.

### 2. Стек Prometheus + Grafana
* **Prometheus** — база данных временных рядов. Он периодически «опрашивает» (scrape) приложения и серверы, забирая у них метрики.
* **Node Exporter** — агент, который собирает системные метрики (CPU, RAM, диск, сеть) с Linux-сервера и отдает их в формате, понятном Prometheus.
* **Grafana** — инструмент визуализации. Он подключается к Prometheus и строит красивые графики (дашборды).

### 3. Архитектура
1. **Node Exporter** запущен на сервере и слушает порт `9100`.
2. **Prometheus** раз в 15 секунд запрашивает `http://node-exporter:9100/metrics` и сохраняет данные.
3. **Grafana** подключается к Prometheus и отображает данные на графиках.

### 4. Развертывание через Docker Compose
Все три компонента удобно запускать вместе:

~~~yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus

  node-exporter:
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin

volumes:
  prometheus_data:
~~~

* `prometheus.yml` — файл конфигурации Prometheus. В нем указывается, какие цели (targets) нужно опрашивать.

### 5. Конфигурация Prometheus (`prometheus.yml`)
~~~yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']
~~~

* `scrape_interval` — как часто опрашивать цели (15 секунд).
* `targets` — список адресов, которые нужно опрашивать.

### 6. Grafana
После запуска Grafana доступна на порту `3000`. 
Логин по умолчанию: `admin`, пароль: `admin` (или тот, что вы указали в `GF_SECURITY_ADMIN_PASSWORD`).
Для начала работы нужно:
1. Добавить источник данных (Data Source) -> Prometheus (URL: `http://prometheus:9090`).
2. Создать дашборд или импортировать готовый (например, Node Exporter Full, ID 1860).

---

## Практика
Переходите к [Справочнику команд](COMMANDS.md) и [Заданиям](TASKS.md).

---

## Чек-лист
- [ ] Понимаю, зачем нужен мониторинг.
- [ ] Знаю, что такое Prometheus, Node Exporter и Grafana.
- [ ] Умею разворачивать стек через Docker Compose.
- [ ] Умею настраивать `prometheus.yml`.
- [ ] Умею подключать Prometheus к Grafana.

---

## Что дальше
На следующем уроке разберем **Введение в Kubernetes (K8s)**.