# Получение категорий статусов

Возвращает справочник категорий статусов.

`GET /cwm/public/api/v1/status-categories`

### Тело успешного ответа 200:

```json
{
  "items": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string"
    }
  ]
}
```

### Описание возвращаемой модели:

| Параметр | Описание                                                        |
| -------- | --------------------------------------------------------------- |
| items    | Список категорий. Идентификаторы и названия категорий статусов. |