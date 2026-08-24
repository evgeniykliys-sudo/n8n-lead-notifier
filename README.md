# n8n-lead-notifier

Сценарий n8n: вебхук с заявкой (например, с формы на сайте) → уведомление в Telegram.

## Запуск
```bash
docker compose up -d
```
Открой http://localhost:5678, создай аккаунт владельца (первый запуск).

Импортировать готовый сценарий: **Workflows → Import from File → `workflow.json`**.
После импорта нужно вручную указать свой Telegram credential (bot token) в ноде Telegram —
он не сохраняется в экспорте по соображениям безопасности.

## Как проверить
```bash
curl -X POST http://localhost:5678/webhook/new-lead \
  -H "Content-Type: application/json" \
  -d '{"name":"Иван Петров","phone":"+7 900 123-45-67","message":"Хочу бота для записи клиентов"}'
```
Сценарий должен прислать отформатированное уведомление в Telegram.

## Что делает
1. **Webhook** — принимает POST-запрос (например, от формы на сайте) с полями name/phone/message
2. **Edit Fields** — собирает данные в читаемый текст
3. **Telegram** — отправляет уведомление в чат
