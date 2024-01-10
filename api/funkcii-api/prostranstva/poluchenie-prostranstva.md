# Получение пространства

Возвращает информацию о конкретном пространстве.

`GET /cwm/public/api/v1/workspaces/{workspace}`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |

### Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "key": "string",
  "name": "string",
  "description": "string",
  "author": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
  }
}
```

### Описание возвращаемой модели пространства:

<table data-header-hidden><thead><tr><th width="263"></th><th></th></tr></thead><tbody><tr><td><strong>Параметр</strong></td><td><strong>Описание</strong></td></tr><tr><td>id</td><td>Идентификатор пространства</td></tr><tr><td>key</td><td>Ключ пространства</td></tr><tr><td>name</td><td>Название пространства</td></tr><tr><td>description</td><td>Описание пространства</td></tr><tr><td>author</td><td><p>Создатель пространства. Идентификатор, отображаемое имя, логин и почта пользователя.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}
</code></pre></td></tr></tbody></table>
