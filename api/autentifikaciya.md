# Аутентификация

TeamStorm REST API использует аутентификацию на основе токена. Это значит, что нужно передавать токен в каждом запросе к API. Токен постоянный, одинаковый для всех запросов.

Нет необходимости генерировать новый токен в каждой сессии.

При аутентификации с использованием токена действует набор привилегий пользователя, выписавшего токен.

```
curl -X POST https://TEAMSTORM_URI/cwm/public/api/v1/... \
 -H 'Authorization: PrivateToken YOUR_TOKEN'
```

&#x20;
