# Удаление задачи

## **Удаление задачи**

`DELETE /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}`

Параметры запроса:

| Параметр  | Обязательность | Описание                                                                                                                                                                                                  |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem  | обязательный   | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |

Ошибки запроса:

<table data-header-hidden><thead><tr><th width="356"></th><th></th></tr></thead><tbody><tr><td>400</td><td><p>Bad Request</p><p>Неправильный (несуществующий) параметр запроса</p></td></tr><tr><td>401</td><td><p>Unauthorized</p><p>Не авторизованный запрос</p></td></tr><tr><td>403</td><td><p>Forbidden</p><p>Отказ доступа к объекту</p></td></tr><tr><td>500</td><td><p>Server Error</p><p>Внутренняя ошибка сервиса</p></td></tr></tbody></table>

Успешный статус запроса 204.
