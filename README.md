# Dev_sprint_9: Настройка стека мониторинга

## Описание
В рамках этого спринта был настроен полноценный стек мониторинга для веб-сервера Nginx.

## Стек технологий
- **Prometheus** (порт 9090) — сбор метрик.
- **Grafana** (порт 3000) — визуализация и алертинг.
- **Loki** (порт 3100) — агрегация логов.
- **Vector** — сбор, парсинг и отправка логов в Loki (с извлечением IP-клиента).
- **Nginx + Nginx Exporter** (порт 9113) — целевой сервис и его метрики.
- **Node Exporter** (порт 9100) — метрики хоста (CPU, RAM).

## Структура репозитория
- `configs/` — конфигурационные файлы сервисов (пароли замаскированы).
- `dashboards/` — JSON-экспорт дашборда "Nginx Overview".
- `alerts/` — JSON-экспорт правил алертинга.
- `screenshots/` — скриншоты, подтверждающие выполнение этапов задания.

## Дашборд "Nginx Overview"
Содержит 6 панелей:
1. Nginx Status (Stat)
2. CPU Load (Time series)
3. RAM Usage (Time series)
4. RPS (Time series)
5. Error Logs (Logs/Table)
6. Top 10 IPs (Pie chart / Bar gauge)

## Алертинг
Настроены два правила с уведомлениями на Email (SMTP Yandex):
1. **NginxDown**: Срабатывает, если `nginx_up < 1` в течение 1 минуты.
2. **InstanceDown**: Срабатывает, если `up{job="node"} < 1` в течение 2 минут.

## Проверка работоспособности
Успешно протестировано:
- Остановка Nginx → алерт NginxDown переходит в статус **Firing** → приходит Email → запуск Nginx → возврат в **Normal**.
- Остановка Node Exporter → алерт InstanceDown переходит в статус **Firing** → приходит Email → запуск Node Exporter → возврат в **Normal**.
