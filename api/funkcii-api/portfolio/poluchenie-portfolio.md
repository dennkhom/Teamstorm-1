# Получение портфолио

Возвращает конкретное портфолио

`GET /cwm/public/api/v1/workspaces/{workspace}/portfolios/{portfolioId}`

Параметры запроса:

| Параметр    | Обязательность | Описание                                                                                                                                                                                                  |
| ----------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace   | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| portfolioId |                | <p>Идентификатор портфолио</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                                    |

Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "description": "string",
  "folder": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  },
  "elements": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string"
    }
  ],
  "workflow": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  }
}
```
