# Состав поставки

Стандартная поставка представляет собой файловый архив вида: `teamstorm_helm_v%Release%.tgz`, где `%Release%` - версия программного обеспечения TeamStorm**.**

Распакуйте архив поставки `teamstorm_helm_v%Release%.tgz`:

```shell
tar -xzvf teamstorm_helm_v%Release%.tgz
```

Содержимое разархивированной директории будет иметь следующую структуру:

```shell
.
├── temp
│   ├── temps
│   │   ├── cwm-networkpolicy.yaml
│   │   ├── deployment.yaml
│   │   ├── hpa.yaml
│   │   └── serviceaccount.yaml
│   ├── ingress.yaml
│   └── service.yaml
├── templates
│   ├── Configmaps    # Конфигурационные файлы для каждого из сервисов
│   │   ├── attachment-configmap.yaml
│   │   ├── blueprint-configmap.yaml
|   |   ├── ...
│   ├── Deployments
│   │   ├── attachment.yaml
│   │   ├── blueprint.yaml
|   |   ├── ...
│   ├── PVs           # Конфигурационные файлы для каждого хранилища
│   │   ├── cwm-rabbitmq-certificates-volume-persistentvolumeclaim.yaml
│   │   ├── cwm-rabbitmq-volume-persistentvolumeclaim.yaml
│   │   ├── database-service-volume-persistentvolumeclaim.yaml
│   │   ├── ssl-volume-persistentvolumeclaim.yaml
│   │   └── trusted-certificates-volume-persistentvolumeclaim.yaml
│   ├── Secrets
│   │   └── registry.yaml
│   ├── Services      # Файлы описания каждого сервиса
│   │   ├── attachment.yaml
│   │   ├── blueprint.yaml
|   |   ├── ...
│   ├── tests
│   │   └── test-connection.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
├── Chart.yaml
└── values.yaml       # Файл с основными параметрами
```
