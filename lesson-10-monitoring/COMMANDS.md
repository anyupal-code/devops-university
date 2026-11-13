# Справочник по мониторингу

## Расшифровка названий
* **Prometheus** — в греческой мифологии титан, подаривший людям огонь. В IT — система мониторинга.
* **Grafana** — *Grafana* (название бренда, не расшифровывается). Инструмент визуализации.
* **Node Exporter** — *Node Exporter* (Экспортер узла). Агент для сбора системных метрик.
* **Scrape** — *Scrape* (Соскребать). Процесс сбора метрик Prometheus'ом.
* **Target** — *Target* (Цель). Адрес, с которого Prometheus собирает метрики.

## Основные команды Docker Compose
* `docker-compose up -d` — запустить стек в фоновом режиме.
* `docker-compose logs -f prometheus` — посмотреть логи Prometheus.
* `docker-compose down` — остановить.

## Работа с Grafana
1. Открыть `http://localhost:3000`.
2. Логин: `admin`, пароль: `admin` (или из конфига).
3. `Configuration` -> `Data Sources` -> `Add data source` -> `Prometheus`.
4. URL: `http://prometheus:9090`.
5. `Save & Test`.

## Импорт дашборда
1. В Grafana: `Dashboards` -> `Import`.
2. Введите ID `1860` (Node Exporter Full).
3. Выберите источник данных Prometheus.
4. Нажмите `Import`.