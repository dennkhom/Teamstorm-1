# Изменение задачи

Изменяет один или несколько параметров задачи. Для изменения атрибутов задачи можно использовать методы изменения атрибутов задачи.

`PATCH /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}`

Параметры запроса:

| Параметр  | Обязательность | Описание                                                                                                                                                                                                  |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem  | обязательный   | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |

Тело запроса:

```json
{
  "name": "string",
  "description": "string",
  "type": "string",
  "workflowId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "status": "string",
  "dueDate": "2023-12-27T11:00:34.853Z",
  "assignee": "string",
  "sprintId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "originalEstimate": 0,
  "parentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "portfolioElementIds": [
    "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  ]
}
```

Параметры тела запроса:

| Параметр              | Обязательность | Описание                                                                                                                                                     |
| --------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `name`                | обязательный   | <p>Название задачи</p><p><code>name: {str:255}</code></p>                                                                                                    |
| `description`         |                | <p>Описание задачи</p><p><code>name: {str:MAX}</code></p>                                                                                                    |
| `type`                | обязательный   | <p>Название или идентификатор типа</p><p><code>type: "Дефект"</code></p><p><code>type: "3fa85f64-5717-4562-b3fc-2c963f66afa6"</code></p>                     |
| `workflowId`          |                | <p>Идентификатор процесса</p><p><code>workflowId: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                          |
| `status`              |                | <p>Название или идентификатор статуса</p><p><code>status: "REQ TEST"</code></p><p><code>status: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>            |
| `dueDate`             |                | <p>Дата выполнения</p><p><code>dueDate: "2022-03-03T21:59:08Z"</code></p>                                                                                    |
| `assignee`            |                | <p>Логин или идентификатор ответственного</p><p><code>assignee: "ivan.ivanov"</code></p><p><code>assignee: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p> |
| `sprintId`            |                | <p>Идентификатор спринта</p><p><code>sprintId: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                             |
| `originalEstimate`    |                | <p>Оценка в секундах</p><p><code>originalEstimate: 56000</code></p>                                                                                          |
| `parentId`            |                | <p>Идентификатор папки или родительской задачи</p><p><code>parent: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                         |
| `portfolioElementIds` |                | <p>Список идентификаторов элементов портфолио</p><p><code>portfolioElementIds: ["b6ac719f-7de5-470a-997c-a83050d26b11"]</code></p>                           |

Ошибки запроса:

| 400 | <p>Bad Request</p><p>Неправильный (несуществующий) параметр запроса</p><p>Неправильная модель тела запроса</p> |
| --- | -------------------------------------------------------------------------------------------------------------- |
| 401 | <p>Unauthorized</p><p>Не авторизованный запрос</p>                                                             |
| 403 | <p>Forbidden</p><p>Отказ доступа к объекту</p>                                                                 |
| 500 | <p>Server Error</p><p>Внутренняя ошибка сервиса</p>                                                            |

Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "key": "string",
  "name": "string",
  "description": "string",
  "type": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  },
  "workflow": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  },
  "status": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string",
    "category": {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string"
    }
  },
  "endDate": "2023-12-27T11:00:34.854Z",
  "createdDate": "2023-12-27T11:00:34.854Z",
  "dueDate": "2023-12-27T11:00:34.854Z",
  "assignee": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
  },
  "author": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
  },
  "sprint": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  },
  "folder": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
  },
  "originalEstimate": 0,
  "timeSpent": 0,
  "remainingEstimate": 0,
  "changedBy": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
  },
  "parent": {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "nodeType": "Folder"
  },
  "attributes": [
    {
      "type": "UniString",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": "string"
    },
    {
      "type": "Number",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": 0
    },
    {
      "type": "Date",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": "2023-12-27T11:00:34.854Z"
    },
    {
      "type": "UniSelect",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string"
      }
    },
    {
      "type": "Tag",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      ]
    },
    {
      "type": "User",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "displayName": "string",
        "username": "string",
        "email": "string"
      }
    },
    {
      "type": "TimeDuration",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": 0
    }
  ],
  "portfolios": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "elements": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      ]
    }
  ],
  "workspace": {
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
}
```

Модель задачи описана в таблице "Описание возвращаемой модели задачи" подраздела "Получение задачи".
