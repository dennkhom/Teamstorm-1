# Получение элемента портфолио

Возвращает конкретный элемент портфолио.

`GET /cwm/public/api/v1/workspaces/{workspace}/portfolio-elements/{portfolioElementId}`

### Параметры запроса:

| Параметр           | Обязательность | Описание                                                                                                                                                                                                  |
| ------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace          | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| portfolioElementId |                | <p>Идентификатор элемента портфолио</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                           |

Возможно комбинирование параметров фильтрации.

### Тело успешного ответа 200:

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

### Описание возвращаемой модели элемента портфолио:

<table data-header-hidden><thead><tr><th width="195"></th><th></th></tr></thead><tbody><tr><td>Параметр</td><td>Описание</td></tr><tr><td>id</td><td>Идентификатор элемента</td></tr><tr><td>name</td><td>Название элемента</td></tr><tr><td>description</td><td>Описание элемента</td></tr><tr><td>startDate</td><td>Плановая дата начала</td></tr><tr><td>endDate</td><td>Плановая дата завершения</td></tr><tr><td>status</td><td><p>Статус элемента. Идентификатор статуса, название статуса, категория статуса.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string",
    "category": {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string"
    }   
}
</code></pre></td></tr><tr><td>responsibles</td><td><p>Список ответственных по элементу. Идентификаторы, отображаемое имя, логин и почта пользователей.</p><pre class="language-json"><code class="lang-json">[{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}]
</code></pre></td></tr><tr><td>portfolio</td><td><p>Портфолио, в которое добавлен элемент. Идентификатор и название портфолио.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr></tbody></table>
