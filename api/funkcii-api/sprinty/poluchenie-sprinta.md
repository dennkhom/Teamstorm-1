# Получение спринта

Возвращает соответствующий спринт.

`GET /cwm/public/api/v1/workspaces/{workspace}/sprints/{sprintId}`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| sprintId     | обязательный       | <p>Идентификатор спринта</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                                      |

### Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "description": "string",
  "startDate": "2024-01-09T12:39:08.894Z",
  "endDate": "2024-01-09T12:39:08.894Z",
  "state": "New",
  "workdays": 0,
  "isBacklog": true,
  "team": [
    {
      "user": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "displayName": "string",
        "username": "string",
        "email": "string"
      },
      "hoursPerDay": 0,
      "daysOff": 0
    }
  ]
}
```

### Описание возвращаемой модели спринта:

<table data-header-hidden><thead><tr><th width="187"></th><th></th></tr></thead><tbody><tr><td><strong>Параметр</strong></td><td><strong>Описание</strong></td></tr><tr><td>id</td><td>Идентификатор спринта</td></tr><tr><td>name</td><td>Название спринта</td></tr><tr><td>description</td><td>Описание спринта</td></tr><tr><td>startDate</td><td>Дата начала спринта</td></tr><tr><td>endDate</td><td>Дата завершения спринта</td></tr><tr><td>state</td><td>Статус спринта (Новый, Активный, Завершенный)</td></tr><tr><td>workdays</td><td>Кол-во рабочих дней в спринте</td></tr><tr><td>isBacklog</td><td>Флаг бэклога. True - элемент является бэклогом, False - элемент является спринтом.</td></tr><tr><td>team</td><td><p>Команда спринта. Список участников с указанием количества рабочих часов в дне и количества дней отпуска.</p><pre class="language-json"><code class="lang-json">[
    {
      "user": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "displayName": "string",
        "username": "string",
        "email": "string"
      },
      "hoursPerDay": 0,
      "daysOff": 0
    }
]
</code></pre></td></tr></tbody></table>
