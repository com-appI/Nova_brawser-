# Nova Browser — подключение как внутренний поиск ChatClaud

Nova работает как отдельный backend-сервис. ChatClaud обращается к нему сервер-сервером через `/api/search`, `/api/fetch-url` и другие `/api/*` маршруты. Не вставляйте `API_TOKEN` в `public/app.js` или HTML.

## Render: Nova Web Service
- Root Directory: пусто (если `package.json` в корне репозитория)
- Build Command: `npm install`
- Start Command: `npm start`

## Environment в Render Nova
- `API_TOKEN`: длинный случайный секрет (32+ случайных байта).
- `SERPER_KEY`: новый Serper API key (обязательно для основного поиска).
- `TAVILY_KEY`: новый Tavily API key (дополнительные ответы/поиск; необязательно).
- `SERPER_KEY_2`: необязательный резервный Serper key.
- `CHATCLAUD_URL`: `https://chatclaud.onrender.com`

`SITE_USER` и `SITE_PASS` необязательны; задавайте их только если хотите закрыть Basic Auth-ом и веб-интерфейс Nova. API защищается `API_TOKEN`. Без `API_TOKEN` или пары `SITE_USER`/`SITE_PASS` API закрыт (fail-closed).

## Environment в Render ChatClaud
- `NOVA_URL`: публичный URL Nova Web Service, например `https://nova-brawser.onrender.com`
- `NOVA_API_TOKEN`: ровно то же значение, что `API_TOKEN` в Nova.

Проверьте `https://ВАШ-NOVA.onrender.com/api/health`: health endpoint публичный и не раскрывает секреты, показывает только наличие настроенных поисковых провайдеров. Затем проверьте внутренний поиск из ChatClaud.

## Важно по безопасности
Предыдущий архив содержал API-ключи прямо в `server.js`. Считайте эти ключи скомпрометированными: отзовите их в кабинетах Serper и Tavily, выпустите новые и добавьте только в Render Environment. Не коммитьте `.env`.
