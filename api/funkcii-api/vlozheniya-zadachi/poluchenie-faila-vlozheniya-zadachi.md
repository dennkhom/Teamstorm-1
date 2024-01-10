# Получение файла вложения задачи

Выгружает файл вложения.

`GET /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}/attachments/{attachmentId}/download`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem     | обязательный       | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |
| attachmentId | обязательный       | <p>Идентификатор вложения</p><p><code>?attachmentId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                       |

Возвращает файл вложения в теле успешного ответа 200.
