# Получение рабочего процесса

Возвращает конкретный рабочий процесс.

`GET /cwm/public/api/v1/workspaces/{workspace}/workflows/{workflow}`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                                                                                              |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| workspace    | обязательный       | <p>Ключ или идентификатор пространства</p><p><code>/cwm/public/api/v1/workspaces/KEY/workitems</code></p><p><code>/cwm/public/api/v1/workspaces/f5ce1753-ced5-4992-beb9-7408c1a56cf8/workitems</code></p> |
| workflow     | обязательный       | <p>Идентификатор или название процесса</p><p><code>Bug Workflow</code></p><p><code>f5ce1753-ced5-4992-beb9-7408c1a56cf8</code></p>                                                                        |

### Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "type": "Workitem",
  "description": "string",
  "transitions": [
    {
      "fromStatus": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string",
        "category": {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      },
      "nextStatus": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string",
        "category": {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      },
      "fromAllStatuses": true,
      "isInitial": true
    }
  ],
  "statuses": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "category": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string"
      }
    }
  ]
}
```

### Описание возвращаемой модели рабочего процесса:

<table data-header-hidden><thead><tr><th width="204"></th><th></th></tr></thead><tbody><tr><td><strong>Параметр</strong></td><td><strong>Описание</strong></td></tr><tr><td>id</td><td>Идентификатор процесса</td></tr><tr><td>name</td><td>Название процесса</td></tr><tr><td>type</td><td>Тип процесса (для задач или для портфеля)</td></tr><tr><td>description</td><td>Описание процесса</td></tr><tr><td>transitions</td><td><p>Список переходов. Начальный статус перехода, конечный статус перехода, флаг возможности перехода в конечный из любого статуса, флаг начального перехода процесса.</p><pre class="language-json"><code class="lang-json">[
    {
      "fromStatus": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string",
        "category": {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      },
      "nextStatus": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string",
        "category": {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "string"
        }
      },
      "fromAllStatuses": true,
      "isInitial": true
    }
]
</code></pre></td></tr><tr><td>statuses</td><td><p>Список статусов в процессе</p><pre class="language-json"><code class="lang-json">[
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "string",
      "category": {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "name": "string"
      }
    }
]
</code></pre></td></tr></tbody></table>
