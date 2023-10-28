# Подготовка к установке

Для обновления TeamStorm до версии 2.61.0 необходимо предварительно обновить TeamStorm до версии 2.33.

Перед обновлением рекомендуется создать резервную копию TeamStorm и Test IT.

Так как работа TeamStorm требует наличие работающего [программного обеспечения Test IT](https://docs.testit.software/installation-guide/install-in-kubernetes.html), то для успешного старта вам нужно внести следующие изменения в конфигурацию:

1.  Установите параметр `CWM_ENABLED: "true"` для `testit-backend/values.yaml`:

    ```yaml
    general:
      config:
        CWM_ENABLED: "true"
    ```
2.  Укажите DNS-имена для следующих служб TeamStorm в файле конфигурации Test IT `testit-frontend/values.yaml`, учитывая имя пространства Kubernetes `teamstorm`:

    ```yaml
    general:
        config:
        CWM_ENABLED: "true"
        CWM_S3_BUCKET_SECRET_KEY: <%YourSecretKey%>
        WIKI_S3_BUCKET_SECRET_KEY: <%YourSecretKey%>
        TASK_TRACKER_GATEWAY_API: "task-tracker-gateway-api.teamstorm:8080"
        WIKI_GATEWAY_API_UPSTREAM: "wiki-gateway-api.teamstorm:8080"
        TASK_TRACKER_WEB_APP: "task-tracker-web-app.teamstorm:8080"
        NOTIFICATION_SERVICE_HUB: "notification-service-hub.teamstorm:8080"
    ```


3. Сгенерируйте значения параметров `CWM_S3_BUCKET_SECRET_KEY` и `WIKI_S3_BUCKET_SECRET_KEY`.\
   Обратите внимание, что недопустимыми символами являются `$:`
4.  Убедитесь в соответствии выставленных значений параметров `CWM_S3_BUCKET_SECRET_KEY` и `WIKI_S3_BUCKET_SECRET_KEY` в файле конфигурации TeamStorm `./teamstorm/values.yml`:

    ```yaml
    general:
        CWM_S3_BUCKET_SECRET_KEY: "MySecretKeyHash"
        WIKI_S3_BUCKET_SECRET_KEY: "MySecretKeyHash"
    ```

