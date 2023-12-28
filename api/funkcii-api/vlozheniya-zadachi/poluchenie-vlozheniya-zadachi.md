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
