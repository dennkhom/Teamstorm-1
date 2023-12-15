# Состав поставки

Стандартная поставка представляет собой файловый архив вида: `teamstorm_helm_v%Release%.tgz`, где `%Release%` - версия программного обеспечения TeamStorm**.**

1. Распакуйте архив поставки `teamstorm_helm_v%Release%.tgz`:

```shell
tar -xzvf teamstorm_helm_v%Release%.tgz
```

2.  Содержимое разархивированных директории будет иметь следующую структуру:&#x20;

    ```
    .
    ├──teamstorm_v2.X # Teamstorm helm charts
    ├──testit_v4.X    # TestIt helm charts
    ```
3. Структура каталога Teamstorm:

```shell
    .
├── temp
│   ├── temps
│   │   ├── cwm-networkpolicy.yaml
│   │   ├── deployment.yaml
│   │   ├── hpa.yaml
│   │   └── serviceaccount.yaml
│   ├── ingress.yaml
│   └── service.yaml
├── templates
│   ├── Configmaps    # Конфигурационные файлы для каждого из сервисов
│   │   ├── attachment-configmap.yaml
│   │   ├── comment-configmap.yaml
|   |   ├── ...
│   ├── Deployments
│   │   ├── attachment.yaml
│   │   ├── comment.yaml
│   ├── Jobs
│   │   ├── attachment.yaml
|   |   ├── ...
│   ├── PVs           # Конфигурационные файлы для каждого хранилища
│   │   ├── cwm-rabbitmq-certificates-volume-persistentvolumeclaim.yaml
│   │   ├── cwm-rabbitmq-volume-persistentvolumeclaim.yaml
│   │   ├── database-service-volume-persistentvolumeclaim.yaml
│   │   ├── ssl-volume-persistentvolumeclaim.yaml
│   │   └── trusted-certificates-volume-persistentvolumeclaim.yaml
│   ├── Secrets
│   │   └── registry.yaml
│   ├── Services      # Файлы описания каждого сервиса
│   │   ├── attachment.yaml
│   │   ├── comment.yaml
|   |   ├── ...
│   ├── tests
│   │   └── test-connection.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
├── Chart.yaml
└── values.yaml       # Файл с основными параметрами

```

4.  Структура каталога Test IT:&#x20;

    ```
    .
    ├── CHANGELOG.md # Changelog
    ├── jobs
    │   ├── minio
    │   │   ├── backup
    │   │   │   ├── configmap.yaml
    │   │   │   └── job.yaml
    │   │   ├── docker-to-k8s
    │   │   │   ├── configmap.yaml
    │   │   │   └── job.yaml
    │   │   ├── export
    │   │   │   ├── configmap.yaml
    │   │   │   └── job.yaml
    │   │   ├── import
    │   │   │   ├── configmap.yaml
    │   │   │   └── job.yaml
    │   │   └── restore
    │   │       ├── configmap.yaml
    │   │       └── job.yaml
    │   └── postgres
    │       ├── backup
    │       │   ├── configmap.yaml
    │       │   └── job.yaml
    │       ├── docker-to-k8s
    │       │   ├── configmap.yaml
    │       │   └── job.yaml
    │       ├── export
    │       │   ├── configmap.yaml
    │       │   └── job.yaml
    │       ├── import
    │       │   ├── configmap.yaml
    │       │   └── job.yaml
    │       └── restore
    │           ├── configmap.yaml
    │           └── job.yaml
    ├── licenses-backend.txt        # Справочная информация о лицензиях
    ├── licenses-frontend.txt
    ├── scripts                     # Каталог со скриптами
    │   ├── 4.1.0-4.2.4_upgrade_plan.yaml
    │   ├── 4.2.4-4.3.1_upgrade_plan.yaml
    │   ├── docker-compose.minio-export.yml
    │   ├── k8s_backup.sh
    │   ├── k8s_minio_migrate.sh
    │   ├── k8s_postgres_migrate.env
    │   ├── k8s_postgres_migrate.sh
    │   ├── k8s_restore.sh
    │   ├── minio-backup.sh
    │   ├── move_to_k8s.sh
    │   └── pre_k8s_backup.sh
    ├── testit_backend              # Каталог с файлами, описания служб
    │   ├── Chart.yaml
    │   ├── templates
    │   │   ├── configmaps          # Конфигурация служб
    │   │   │   ├── appsettings
    │   │   │   │   ├── auth.yaml
    │   │   │   │   ├── ...
    │   │   │   │   └── webapi.yaml
    │   │   │   ├── auth.yaml
    │   │   │   ├── ...
    │   │   │   ├── ssl
    │   │   │   │   ├── auth-cache-ssl.yaml
    │   │   │   │   ├── ca-bundle.yaml
    │   │   │   │   ├── influxdb-ssl.yaml
    │   │   │   │   ├── minio-ssl.yaml
    │   │   │   │   ├── postgres-ssl.yaml
    │   │   │   │   └── rabbitmq-ssl.yaml
    │   │   │   └── webapi.yaml
    │   │   ├── deployments
    │   │   │   ├── auth.yaml
    │   │   │   ├── ...
    │   │   │   └── webapi.yaml
    │   │   ├── services
    │   │   │   ├── auth-cache.yaml
    │   │   │   ├── ...
    │   │   │   └── webapi.yaml
    │   │   └── statefulsets
    │   │       ├── auth-cache.yaml
    │   │       ├── influxdb.yaml
    │   │       ├── minio.yaml
    │   │       ├── postgres.yaml
    │   │       └── rabbitmq.yaml
    │   ├── values-override.yaml
    │   ├── values-ssl.yaml
    │   └── values.yaml
    └── testit_frontend             # Конфигурация веб-сервера
        ├── Chart.yaml
        ├── templates
        │   ├── configmaps
        │   │   └── frontend.yaml
        │   ├── deployments
        │   │   └── frontend.yaml
        │   └── services
        │       ├── frontend.yaml
        │       └── ingress.yaml
        ├── values-override.yaml
        ├── values-ssl.yaml
        └── values.yaml             # Основной конфигурационный файл
    ```
