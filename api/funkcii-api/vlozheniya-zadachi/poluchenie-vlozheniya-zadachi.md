# Получение вложения задачи

Возвращает метаданные вложения задачи.

`GET /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}/attachments/{attachmentId}`

| Параметр     | Обязательность | Описание                                                                                                                                                                                                  |
| ------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem     | обязательный   | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |
| attachmentId | обязательный   | <p>Идентификатор вложения</p><p><code>?attachmentId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                       |

Тело успешного ответа 200:

```json
{
  "attachmentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "workspaceId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "createdBy": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
  },
  "fileId": "string",
  "name": "string",
  "type": "string",
  "size": 0,
  "createdAt": "2023-12-27T14:21:46.574Z"
}
```

Описание возвращаемой модели вложения:

<table data-header-hidden><thead><tr><th width="260"></th><th></th></tr></thead><tbody><tr><td>Параметр</td><td>Описание</td></tr><tr><td>attachmentId</td><td>Идентификатор вложения</td></tr><tr><td>workspaceId</td><td>Идентификатор пространства, в которое добавлено вложение</td></tr><tr><td>createdBy</td><td><p>Автор вложения. Идентификатор, отображаемое имя, логин и почта пользователя.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}
</code></pre></td></tr><tr><td>fileId</td><td>Идентификатор файла вложения</td></tr><tr><td>name</td><td>Название файла вложения</td></tr><tr><td>type</td><td>MIME-тип файла вложения</td></tr><tr><td>size</td><td>Размер файла вложения в байтах</td></tr><tr><td>createdAt</td><td>Время создания вложения</td></tr></tbody></table>
