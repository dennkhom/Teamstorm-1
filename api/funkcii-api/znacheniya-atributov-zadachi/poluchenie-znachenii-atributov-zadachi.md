# Получение значений атрибутов задачи

Возвращает список заполненных атрибутов задачи.

`GET /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}/attributes`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem     | обязательный       | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |

### Тело успешного ответа 200:

```json
{
  "items": [
    {
      "type": "UniString",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": "string"
    },
    {
      "type": "Number",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": 0
    },
    {
      "type": "Date",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": "2023-12-27T11:13:00.074Z"
    },
    {
      "type": "UniSelect",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string"
      }
    },
    {
      "type": "Tag",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      ]
    },
    {
      "type": "User",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "displayName": "string",
        "username": "string",
        "email": "string"
      }
    },
    {
      "type": "TimeDuration",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": 0
    }
  ]
}
```

### Описание возвращаемой модели списка атрибутов:

<table data-header-hidden><thead><tr><th width="214"></th><th></th></tr></thead><tbody><tr><td><strong>Параметр</strong></td><td><strong>Описание</strong></td></tr><tr><td>items</td><td><p>Список заполненных атрибутов задачи. Тип атрибута, идентификатор, название атрибута, описание атрибута, значение атрибута (формат данных описан в разделе Описание моделей значений атрибутов)</p><pre class="language-json"><code class="lang-json">[{
      "type": "UniString",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": "string"
}]
</code></pre></td></tr></tbody></table>
