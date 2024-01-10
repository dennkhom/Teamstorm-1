# Добавление нового комментария к задаче

Добавляет новый комментарий в задаче

`POST /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}/comments`

Параметры запроса:

| Параметр  | Обязательность | Описание                                                                                                                                                                                                  |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem  | обязательный   | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |

Тело запроса:

```json
{
  "text": "string"
}
```

Параметры тела запроса:

| Параметр | Обязательность | Описание                                                      |
| -------- | -------------- | ------------------------------------------------------------- |
| text     | обязательный   | <p>Тело комментария</p><p><code>text: {string:MAX}</code></p> |

Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "text": "string",
  "author": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
  },
  "createdAt": "2023-12-27T14:06:04.915Z"
}
```

Описание возвращаемой модели комментария:

<table data-header-hidden><thead><tr><th width="220"></th><th></th></tr></thead><tbody><tr><td>Параметр</td><td>Описание</td></tr><tr><td>id</td><td>Идентификатор комментария</td></tr><tr><td>text</td><td>Секс комментария. Формат разметки комментария описан в разделе Форматирование текста</td></tr><tr><td>author</td><td><p>Автор комментария. Идентификатор, отображаемое имя, логин и почта пользователя.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}
</code></pre></td></tr><tr><td>createdAt</td><td>Время создания комментария</td></tr></tbody></table>
