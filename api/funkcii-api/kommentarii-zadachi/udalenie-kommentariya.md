# Удаление комментария

Удаляет комментарий.

`DELETE /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}/comments/{commentId}`

### Параметры запроса:

| Параметр  | Обязательность | Описание                                                                                                                                                                                                  |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem  | обязательный   | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |
| commentId | обязательный   | <p>Идентификатор комментария</p><p><code>?commentId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                       |

Успешный статус запроса 204.
