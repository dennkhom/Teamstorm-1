# Получение элемента портфолио

Возвращает конкретный элемент портфолио.

`GET /cwm/public/api/v1/workspaces/{workspace}/portfolio-elements/{portfolioElementId}`

Параметры запроса (возможно комбинирование параметров фильтрации):

| Параметр           | Обязательность | Описание                                                                                                                                                                                                  |
| ------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace          | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| portfolioElementId |                | <p>Идентификатор элемента портфолио</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                           |

Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "description": "string",
  "startDate": "2023-12-29T11:03:51.986Z",
  "endDate": "2023-12-29T11:03:51.986Z",
  "status": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string",
    "category": {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string"
    }
  },
  "responsibles": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "displayName": "string",
      "username": "string",
      "email": "string"
    }
  ],
  "portfolio": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  }
}
```
