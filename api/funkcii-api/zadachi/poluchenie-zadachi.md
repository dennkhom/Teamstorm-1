# Получение задачи

Возвращает конкретную задачу. В модели задачи возвращаются только заполненные атрибуты.

`GET /cwm/public/api/v1/workspaces/{workspace}/workitems/{workitem}`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workitem     | обязательный       | <p>Ключ или идентификатор задачи</p><p><code>?workitem=TS-13</code></p><p><code>?workitem=f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                 |

### Ошибки запроса:

| 400 | <p>Bad Request</p><p>Неправильный (несуществующий) параметр запроса</p> |
| --- | ----------------------------------------------------------------------- |
| 401 | <p>Unauthorized</p><p>Не авторизованный запрос</p>                      |
| 403 | <p>Forbidden</p><p>Отказ доступа к объекту</p>                          |
| 500 | <p>Server Error</p><p>Внутренняя ошибка сервиса</p>                     |

### Тело успешного ответа 200:

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
  "endDate": "2023-12-27T10:29:43.385Z",
  "createdDate": "2023-12-27T10:29:43.385Z",
  "dueDate": "2023-12-27T10:29:43.385Z",
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
      "value": "2023-12-27T10:29:43.385Z"
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

### Описание возвращаемой модели задачи:

<table data-header-hidden><thead><tr><th width="219"></th><th></th></tr></thead><tbody><tr><td><strong>Параметр</strong></td><td><strong>Описание</strong></td></tr><tr><td>id</td><td>Идентификатор задачи</td></tr><tr><td>key</td><td>Ключ задачи</td></tr><tr><td>name</td><td>Название задачи</td></tr><tr><td>description</td><td>Описание задачи. Формат разметки описания приведен в разделе Форматирование текста</td></tr><tr><td>type</td><td><p>Тип задачи. Идентификатор и название типа.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
 }
</code></pre></td></tr><tr><td>workflow</td><td><p>Рабочий процесс задачи. Идентификатор и название процесса.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr><tr><td>status</td><td><p>Статус задачи. Идентификатор, название и категория статуса.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string",
    "category": {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string"
    }
}
</code></pre></td></tr><tr><td>endDate</td><td>Дата фактического завершения задачи</td></tr><tr><td>createdDate</td><td>Дата создания задачи</td></tr><tr><td>dueDate</td><td>Плановая дата выполнения задачи</td></tr><tr><td>assignee</td><td><p>Ответственный по задаче. Идентификатор, отображаемое имя, логин и почта пользователя.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}
</code></pre></td></tr><tr><td>author</td><td><p>Автор задачи. Идентификатор, отображаемое имя, логин и почта пользователя.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}
</code></pre></td></tr><tr><td>sprint</td><td><p>Спринт, в котором находится задача. Идентификатор и название спринта.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr><tr><td>folder</td><td><p>Папка, в которой находится задача. Идентификатор и название папки.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "string"
}
</code></pre></td></tr><tr><td>originalEstimate</td><td>Оценка задачи в секундах</td></tr><tr><td>timeSpent</td><td>Суммарное затраченное время по задаче в секундах</td></tr><tr><td>remainingEstimate</td><td>Оставшееся время в секундах (разница между оценкой и затраченным временем)</td></tr><tr><td>changedBy</td><td><p>Автор последнего изменения задачи. Идентификатор, отображаемое имя, логин и почта пользователя.</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "displayName": "string",
    "username": "string",
    "email": "string"
}
</code></pre></td></tr><tr><td>parent</td><td><p>Родительская задача или папка. Идентификатор и тип элемента (Folder или WorkItem).</p><pre class="language-json"><code class="lang-json">{
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "nodeType": "Folder"
}
</code></pre></td></tr><tr><td>attributes</td><td><p>Список заполненных атрибутов задачи. Тип атрибута, идентификатор, название атрибута, описание атрибута, значение атрибута (формат данных описан в разделе Описание моделей значений атрибутов)</p><pre class="language-json"><code class="lang-json">[{
      "type": "UniString",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "description": "string",
      "value": "string"
}]
</code></pre></td></tr><tr><td>portfolios</td><td><p>Список элементов портфелей, в которые добавлена задача. Идентификатор портфеля, название портфеля, список идентификаторов и названий элементов портфеля.</p><pre class="language-json"><code class="lang-json">[{
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "elements": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      ]
}]
</code></pre></td></tr><tr><td>workspace</td><td><p>Пространство, в котором создана задача. Идентификатор пространства, ключ пространства, название пространства, описание пространтства, автор пространства.</p><pre class="language-json"><code class="lang-json">{
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
</code></pre></td></tr></tbody></table>
