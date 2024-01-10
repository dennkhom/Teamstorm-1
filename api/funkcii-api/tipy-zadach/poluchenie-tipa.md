# Получение типа

Возвращает конкретный тип задачи.

`GET /cwm/public/api/v1/workspaces/{workspace}/types/{type}`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| type         | обязательный       | <p>Идентификатор или название типа</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p><p><code>Задача</code></p>                                                                                  |

### Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "icon": {
    "color": "Tomato",
    "icon": "string"
  },
  "workflow": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  },
  "attributes": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "type": "UniString",
      "options": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      ]
    }
  ]
}
```

### Описание возвращаемой модели типа задачи:

<table data-header-hidden><thead><tr><th width="202"></th><th></th></tr></thead><tbody><tr><td><strong>Параметр</strong></td><td><strong>Описание</strong></td></tr><tr><td>id</td><td>Идентификатор типа</td></tr><tr><td>name</td><td>Название типа</td></tr><tr><td>icon</td><td><p>Иконка типа. Цвет и наименование иконки</p><pre class="language-json"><code class="lang-json">{
    "color": "Tomato",
    "icon": "string"
}
</code></pre></td></tr><tr><td>workflow</td><td><p>Рабочий процесс по умолчанию для типа. Идентификатор и название процесса.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr><tr><td>attributes</td><td>Список атрибутов. Модель атрибута описана в таблице "Описание возвращаемой модели атрибута"</td></tr></tbody></table>
