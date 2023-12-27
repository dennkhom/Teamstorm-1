# Создание задачи

## **Создание задачи**

`POST /cwm/public/api/v1/workspaces/{workspace}/workitems`

Параметры запроса:

| Параметр  | Обязательность | Описание                                                                                                                                                                                                  |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |

Тело запроса:

```json
{
  "name": "string",
  "description": "string",
  "type": "string",
  "workflow": "string",
  "status": "string",
  "dueDate": "2023-12-27T10:33:28.999Z",
  "assignee": "string",
  "sprintId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "originalEstimate": 0,
  "parentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "attributes": [
    {
      "type": "UniString",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": "string"
    },
    {
      "type": "Number",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": 0
    },
    {
      "type": "Date",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": "2023-12-27T10:33:28.999Z"
    },
    {
      "type": "UniSelect",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": "string"
    },
    {
      "type": "Tag",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": [
        "string"
      ]
    },
    {
      "type": "User",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "userName": "string"
      }
    },
    {
      "type": "TimeDuration",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "value": 0
    }
  ],
  "portfolioElementIds": [
    "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  ]
}
```

Параметры тела запроса:

| Параметр              | Обязательность | Описание                                                                                                                                                                                                                                                                           |
| --------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                | обязательный   | <p>Название задачи</p><p><code>name: {str:255}</code></p>                                                                                                                                                                                                                          |
| `description`         |                | <p>Описание задачи</p><p><code>name: {str:MAX}</code></p>                                                                                                                                                                                                                          |
| `type`                | обязательный   | <p>Название или идентификатор типа</p><p><code>type: "Дефект"</code></p><p><code>type: "3fa85f64-5717-4562-b3fc-2c963f66afa6"</code></p>                                                                                                                                           |
| `workflow`            |                | <p>Название или идентификатор процесса</p><p><code>workflow: "Процесс для дефектов"</code></p><p><code>workflow: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                                                                                 |
| `status`              |                | <p>Название или идентификатор статуса</p><p><code>status: "REQ TEST"</code></p><p><code>status: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                                                                                                  |
| `dueDate`             |                | <p>Дата выполнения</p><p><code>dueDate: "2022-03-03T21:59:08Z"</code></p>                                                                                                                                                                                                          |
| `assignee`            |                | <p>Логин или идентификатор ответственного</p><p><code>assignee: "ivan.ivanov"</code></p><p><code>assignee: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                                                                                       |
| `sprintId`            |                | <p>Идентификатор спринта</p><p><code>sprintId: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                                                                                                                                                   |
| `originalEstimate`    |                | <p>Оценка в секундах</p><p><code>originalEstimate: 56000</code></p>                                                                                                                                                                                                                |
| `parentId`            |                | <p>Идентификатор папки или родительской задачи</p><p><code>parent: "b6ac719f-7de5-470a-997c-a83050d26b11"</code></p>                                                                                                                                                               |
| `attributes`          |                | <p>Список кастомных атрибутов (идентификатор + значение атрибута)</p><p><code>[{</code></p><p>    <code>"id": "b6ac719f-7de5-470a-997c-a83050d26b11",</code></p><p>    <code>"value": {}</code></p><p><code>}]</code></p><p>Описание моделей значений атрибутов приведено ниже</p> |
| `portfolioElementIds` |                | <p>Список идентификаторов элементов портфолио</p><p><code>portfolioElementIds: ["b6ac719f-7de5-470a-997c-a83050d26b11"]</code></p>                                                                                                                                                 |

\
Описание моделей значений атрибутов:

| Тип атрибута   | Формат                               | Пример                                                                                                                                   |
| -------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `UniString`    | Строка {str:255}                     | `"value": "Some string"`                                                                                                                 |
| `Number`       | Число                                | `"value": -145.454443435345454`                                                                                                          |
| `Date`         | Дата/время в формате ISO             | `"value": "2022-03-03T21:59:08Z"`                                                                                                        |
| `UniSelect`    | Значение опции из списка             | `"value": "Medium"`                                                                                                                      |
| `Tag`          | Список опций тега                    | `"value": ["tag 1", "tag 2"]`                                                                                                            |
| `User`         | Идентификатор или логин пользователя | <p><code>"value": {"username": "roman.cherepanov"}</code></p><p><code>"value": {"id": "b6ac719f-7de5-470a-997c-a83050d26b11"}</code></p> |
| `TimeDuration` | Время в секундах (целое)             | `"value": 56000`                                                                                                                         |

Ошибки запроса:

| 400 | <p>Bad Request</p><p>Неправильный (несуществующий) параметр запроса</p><p>Неправильная модель тела запроса</p><p>Отсутствие обязательного параметра в модели тела запроса</p> |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 401 | <p>Unauthorized</p><p>Не авторизованный запрос</p>                                                                                                                            |
| 403 | <p>Forbidden</p><p>Отказ доступа к объекту</p>                                                                                                                                |
| 500 | <p>Server Error</p><p>Внутренняя ошибка сервиса</p>                                                                                                                           |

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
  "endDate": "2023-12-27T10:40:03.951Z",
  "createdDate": "2023-12-27T10:40:03.951Z",
  "dueDate": "2023-12-27T10:40:03.951Z",
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
      "value": "2023-12-27T10:40:03.951Z"
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
