# Архитектура: n8n-lead-notifier

## Цель (1 предложение)
n8n-сценарий: вебхук (заявка с сайта/формы) → форматирование данных → уведомление в Telegram.

## Модули
| Модуль | Файл | Описание |
|--------|------|----------|
| Инфраструктура | docker-compose.yml | self-hosted n8n через Docker, именованный volume для данных (workflows, credentials, история выполнений) |
| Сценарий | workflow.json | Экспорт готового workflow: Webhook → Edit Fields → Telegram |

## Стек
- n8n (self-hosted, Docker)
- Node: Webhook (HTTP POST триггер)
- Node: Edit Fields / Set (форматирование сообщения)
- Node: Telegram (отправка уведомления)

## Схема
POST /webhook/new-lead (JSON: name, phone, message) → Edit Fields собирает читаемый текст → Telegram-нода отправляет сообщение в чат

## Заметки
- Credential Telegram создан через n8n REST API (POST /api/v1/credentials), токен не хранится в workflow.json — безопасно для публикации.
- Сценарий создан и активирован через n8n REST API (POST /api/v1/workflows, POST /api/v1/workflows/{id}/activate), а не только через UI — рабочий способ деплоить сценарии клиенту программно/массово.
- Проверено сквозным тестом: POST на вебхук → execution status "success" → сообщение дошло в Telegram.
- `N8N_SECURE_COOKIE=false` в docker-compose нужен, потому что n8n по умолчанию ставит Secure-cookie (только HTTPS) — без этой настройки логин не работает на http://localhost. На проде за HTTPS эту настройку убрать.
