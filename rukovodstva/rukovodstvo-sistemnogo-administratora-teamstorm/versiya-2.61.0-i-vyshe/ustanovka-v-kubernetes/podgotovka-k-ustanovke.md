# Подготовка к установке

Для обновления TeamStorm до версии 2.61.0 и выше необходимо предварительно обновить TeamStorm до версии 2.33.

Перед обновлением рекомендуется создать резервную копию TeamStorm и Test IT.

Так как работа TeamStorm требует наличие работающего [программного обеспечения Test IT](https://docs.testit.software/installation-guide/install-in-kubernetes.html), то для успешного старта вам нужно внести следующие изменения в конфигурацию:

1. Убедитесь в том, что параметр `CWM_ENABLED: "true"` для `testit-backend/values.yaml` и `testit-frontend/values.yaml`:

```
general:
  config:
    CWM_ENABLED: "true"
```

2. Укажите DNS имена для следующих служб **TeamStorm** в файле конфигурации **Test IT** `testit-frontend/values.yaml`, учитывая имя пространства Kubernetes `teamstorm`:

```
general:
  config:
    CWM_ENABLED: "true"
    CWM_S3_BUCKET_NAME: "cwm"
    CWM_S3_BUCKET_SECRET_KEY: "secretKey"
    WIKI_S3_BUCKET_NAME: "wiki"
    WIKI_S3_BUCKET_SECRET_KEY: "secretKey"
    TASK_TRACKER_GATEWAY_API: "task-tracker-gateway-api.teamstorm:8080"
    WIKI_GATEWAY_API_UPSTREAM: "wiki-gateway-api.teamstorm:8080"
    TASK_TRACKER_WEB_APP: "task-tracker-web-app.teamstorm:8080"
    NOTIFICATION_SERVICE_HUB: "notification-service-hub.teamstorm:8080"
    CWM_PUBLIC_GATEWAY_API: "cwm-public-gateway-api.teamstorm:8080"
```

3. &#x20;Сгенерируйте значения параметров `CWM_S3_BUCKET_SECRET_KEY` и `WIKI_S3_BUCKET_SECRET_KEY`. Обратите внимание, что недопустимым символами являются `$`:
4. Убедитесь в соответствии выставленных значений параметров `CWM_S3_BUCKET_SECRET_KEY` и `WIKI_S3_BUCKET_SECRET_KEY` в файлах конфигурации **TeamStorm** и **Test IT** `values.yml`:



`teamstorm/values.yml`:

```
main:
  ...
  minio:
    ...
    cwm_s3_bucket_secret_key: "secretKey"
    wiki_s3_bucket_secret_key: "secretKey"
```

`testit-frontend/values.yaml`

```
general:
  config:
    ...
    CWM_S3_BUCKET_SECRET_KEY: "secretKey"
    WIKI_S3_BUCKET_SECRET_KEY: "secretKey"
    ...
```

5. Параметры секции `main.tms` конфигурационного файла **TeamStorm** должны указывать на соответствующие сервисы **Test IT**

```
main:
...
  tms:
    auth_url: "http://auth.testit:8080"
    auth_cache_url: "auth-cache.testit"
    avatars_api_url: "http://avatars-api.testit:8080/api/"
    license_url: "http://license-service.testit:8080"
    s3_endpoint_url: "http://minio.testit:9000"
    s3_access_key: "testitAccessKey"
    s3_secret_key: "testitSecretKey"
    use_auth_openid: "false"
...
```



{% hint style="warning" %}
Конфигурационные файлы предустановлены с учётом использования пространств имен `testit` и `teamstorm`. В случае их изменения требуется отразить это в конфигурационных файлах.
{% endhint %}
