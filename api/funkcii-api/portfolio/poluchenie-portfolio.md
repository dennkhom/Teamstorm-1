# Получение портфолио

Возвращает конкретное портфолио.

`GET /cwm/public/api/v1/workspaces/{workspace}/portfolios/{portfolioId}`

### Параметры запроса:

| Параметр    | Обязательность | Описание                                                                                                                                                                                                  |
| ----------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace   | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| portfolioId |                | <p>Идентификатор портфолио</p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                                    |

### Тело успешного ответа 200:

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

### Описание возвращаемой модели портфолио:

<table data-header-hidden><thead><tr><th width="264"></th><th></th></tr></thead><tbody><tr><td>Параметр</td><td>Описание</td></tr><tr><td>id</td><td>Идентификатор портфолио</td></tr><tr><td>name</td><td>Название портфолио</td></tr><tr><td>description</td><td>Описание портфолио</td></tr><tr><td>folder</td><td><p>Папка, в которую добавлено портфолио. Идентификатор и название папки.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr><tr><td>elements</td><td><p>Список элементов портфолио. Название и идентификатор элемента.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr><tr><td>workflow</td><td><p>Процесс, определяющий статусную модель портфолио. Название и идентификатор процесса.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr></tbody></table>
