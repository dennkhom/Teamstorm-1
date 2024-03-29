# Получение пользователя

Возвращает информацию о конкретном пользователе.

`GET /cwm/public/api/v1/users/{user}`

### Параметры запроса:

| **Параметр** | **Обязательность** | **Описание**                                                                                                                       |
| ------------ | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| user         | обязательный       | <p>Логин или идентификатор пользователя</p><p><code>ivan.ivanov</code></p><p><code>3fa85f64-5717-4562-b3fc-2c963f66afa6</code></p> |

### Тело успешного ответа 200:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "displayName": "string",
  "username": "string",
  "email": "string"
}
```

### Описание возвращаемой модели пользователя:

| **Параметр** | **Описание**                  |
| ------------ | ----------------------------- |
| id           | Идентификатор пользователя    |
| displayName  | Отображаемое имя пользователя |
| username     | Логин пользователя            |
| email        | Почтовый адрес пользователя   |
