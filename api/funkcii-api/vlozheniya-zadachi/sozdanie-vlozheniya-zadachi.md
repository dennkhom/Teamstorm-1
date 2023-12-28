# Создание вложения задачи

Добавляет вложение в задачу, файл которого предварительно загружено на сервер.

`POST /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}/attachments/{attachmentId}`

Параметры запроса:

| Параметр     | Обязательность | Описание                                                                                                                                                                                                  |
| ------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem     | обязательный   | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |
| attachmentId | обязательный   | <p>Идентификатор вложения, генерируется клиентом</p><p><code>?attachmentId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                |

Тело запроса:

```json
{
  "fileName": "string",
  "contentType": "string",
  "contentLength": 0
}
```

Параметры тела запроса:

| Параметр      | Обязательность | Описание                                                                    |
| ------------- | -------------- | --------------------------------------------------------------------------- |
| fileName      | обязательный   | <p>Название файла</p><p><code>fileName: {string:255}</code></p>             |
| contentType   | обязательный   | <p>Тип вложения (MIME-type)</p><p><code>contentType: "image/png"</code></p> |
| contentLength | обязательный   | <p>Размер вложения в байтах</p><p><code>contentLength: 100500</code></p>    |

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
  "createdAt": "2023-12-27T14:49:27.680Z"
}
```
