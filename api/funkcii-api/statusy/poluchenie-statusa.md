# Получение статуса

Возвращает конкретный статус

`GET /cwm/public/api/v1/workspaces/{workspace}/statuses/{status}`

Параметры запроса:

| Параметр  | Обязательность | Описание                                                                                                                                                                                                  |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| status    | обязательный   | <p>Идентификатор или название статуса</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p><p><code>Done</code></p>                                                                                 |

Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "category": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  }
}
```

Описание возвращаемой модели статуса:

<table data-header-hidden><thead><tr><th width="161"></th><th></th></tr></thead><tbody><tr><td>Параметр</td><td>Описание</td></tr><tr><td>id</td><td>Идентификатор спринта</td></tr><tr><td>name</td><td>Название статуса</td></tr><tr><td>category</td><td><p>Категория статуса. Идентификатор и название категории.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr></tbody></table>
