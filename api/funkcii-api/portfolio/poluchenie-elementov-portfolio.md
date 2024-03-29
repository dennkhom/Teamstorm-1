# Получение элементов портфолио

Возвращает пагинированный список элементов портфолио по указанным параметрам.

`GET /cwm/public/api/v1/workspaces/{workspace}/portfolio-elements`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| name         |                    | <p>Имя элемента для поиска (вхождение подстроки)</p><p><code>?name=Версия 1</code></p>                                                                                                                    |
| folderId     |                    | <p>Идентификатор содержащей папки</p><p><code>?folderId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                   |
| portfolioId  |                    | <p>Идентификатор портфолио</p><p><code>?portfolioId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                       |
| status       |                    | <p>Название или идентификатор статуса элемента</p><p><code>?status=Под риском</code></p><p><code>?status=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                  |

Возможно комбинирование параметров фильтрации.

### Тело успешного ответа 200:

```json
{
  "items": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "startDate": "2023-12-29T10:52:30.494Z",
      "endDate": "2023-12-29T10:52:30.494Z",
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
  ]
}
```

### Описание возвращаемой модели:

| **Параметр** | **Описание**                                                                                                                                              |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| items        | Список элементов. Модель элемента портфолио описана в таблице "Описание возвращаемой модели элемента портфолио" подраздела "Получение элемента портфолио" |
