# Получение списка задач в пространстве с фильтрацией и пагинацией

Возвращает список задач в пространстве, соответствующих параметрам запроса. В модели задачи возвращаются только заполненные атрибуты.

`GET /cwm/public/api/v1/workspaces/{workspace}/workitems`

Параметры запроса:

Возможно комбинирование нескольких параметров фильтрации

| Параметр           | Обязательность | Описание                                                                                                                                                                                                  |
| ------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace          | обязательный   | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| type               |                | <p>Название или идентификатор типа (точное соответствие)</p><p><code>?type=User%20Story</code></p><p><code>?type=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                          |
| parent             |                | <p>Ключ или идентификатор родительского элемента (задача или папка)</p><p><code>?parent=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p><p><code>?parent=TS-15</code></p>                                  |
| sprintId           |                | <p>Идентификатор спринта</p><p><code>?sprintId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                                            |
| portfolioElementId |                | <p>Идентификатор элемента портфолио</p><p><code>?portfolioElementId=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                       |
| name               |                | <p>Имя задачи (поиск по вхождению подстроки)</p><p><code>?name={string:255}</code></p>                                                                                                                    |
| assignee           |                | <p>Логин или идентификатор ответственного</p><p><code>?assignee=ivan.ivanov</code></p><p><code>?assignee=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                  |
| author             |                | <p>Логин или идентификатор создателя</p><p><code>?author=ivan.ivanov</code></p><p><code>?author=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                           |
| status             |                | <p>Название или идентификатор статуса</p><p><code>?status=REQ%20TEST</code></p><p><code>?status=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                           |
| statusCategory     |                | <p><code>?statusCategory=В работе</code></p><p><code>?statusCategory=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                                      |
| fromToken          |                | <p>Токен запрашиваемой страницы</p><p><code>?fromToken=f5ce1753</code></p>                                                                                                                                |
| maxItemsCount      |                | <p>Максимальное кол-во задач на странице (по умолчанию: 50)</p><p><code>?maxItemsCount=200</code></p>                                                                                                     |

Ошибки запроса:

| 400 | <p>Bad Request</p><p>Неправильный (несуществующий) параметр запроса</p><p>Неправильное значение параметра запроса, неправильный формат значения</p><p>Дублирование параметра запроса</p> |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 401 | <p>Unauthorized</p><p>Не авторизованный запрос</p>                                                                                                                                       |
| 403 | <p>Forbidden</p><p>Отказ доступа к объекту</p>                                                                                                                                           |
| 500 | <p>Server Error</p><p>Внутренняя ошибка сервиса</p>                                                                                                                                      |

Тело успешного ответа 200:

```json
{
  "fromToken": "string",
  "maxItemsCount": 0,
  "nextToken": "string",
  "items": [
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
      "endDate": "2023-12-27T09:49:35.712Z",
      "createdDate": "2023-12-27T09:49:35.712Z",
      "dueDate": "2023-12-27T09:49:35.712Z",
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
          "value": "2023-12-27T09:49:35.713Z"
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
  ]
}
```

Описание возвращаемой модели:

| Параметр      | Описание                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------- |
| fromToken     | Токен текущей страницы результатов                                                                                  |
| maxItemsCount | Кол-во запрошенных элементов на странице результатов                                                                |
| nextToken     | Токен следующей страницы результатов                                                                                |
| items         | Список задач. Модель задачи описана в таблице "Описание возвращаемой модели задачи" в подразделе "Получение задачи" |
