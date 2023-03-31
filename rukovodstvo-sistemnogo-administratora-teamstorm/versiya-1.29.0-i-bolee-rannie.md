# Версия 1.29.0 и более ранние

## Назначение документа

Документ описывает действия системного администратора по установке и настройке ПО TeamStorm версии 1.29.0 и ниже.

## **Установка программного ПО**

### Установка, перезапуск и удаление в Docker Compose&#x20;

#### **Требования**

​[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)​

Docker Engine 20.10.17 и выше

Docker Compose 1.17.0 и выше

#### **Состав поставки**

images.tar - архив с образами (только в архиве для автономной установки)**.** Установка TeamStorm выполняется вместе с установкой Test IT.

Состав поставки Test IT указан в [документации на ПО Test IT](https://docs.testit.software/installation-guide/ustanovka-v-docker-compose.html#%D1%81%D0%BE%D1%81%D1%82%D0%B0%D0%B2-%D0%BF%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%BA%D0%B8).

Состав поставки TeamStorm:

* `setup.sh` - основной скрипт установки
* `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам Teamstorm
* `docker-compose.yml` - конфигурационный файл Docker Compose

#### **Подготовка**

1. Измените дефолтные значения переменных в .env-файле.
2.  Задайте параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:

    ```bash
    echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
    sysctl -p
    ```
3. Заблокируйте все порты, кроме порта 80, необходимого для доступа к пользовательскому интерфейсу.
4.  **Опционально:** для обслуживания системы посредством протокола SSH, необходимо открыть порт 22 (может быть переназначено на конкретной конфигурации). Для работы по HTTPS необходимо открыть порт 443. Пример открытия доступа к портам для CentOS 7:

    ```bash
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    firewall-cmd --zone=public --add-port=22/tcp --permanent
    firewall-cmd --zone=public --add-port=443/tcp --permanent
    firewall-cmd --reload
    ```

#### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.&#x20;

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v0.16.0`.
2. Выполните следующие команды:

```bash
tar -xzvf ts_distro_v0.16.0.tgz
chmod +x setup.sh
./setup.sh
```

#### **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

```bash
cd teamstorm_v0.16.0
docker-compose -f docker-compose.yml --project-name teamstorm restart --timeout 120
```

#### Удаление системы

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```bash
cd teamstorm_v0.16.0
docker-compose -f docker-compose.yml --project-name teamstorm down --volumes --timeout 120
```

Чтобы сохранить информацию для последующего использования, выполните команду без флага `--volumes`.

## Описание .env файла

Репозиторий для скачивания образов установки Teamstorm:

```
DOCKER_REGISTRY=docker.testit.ru/teamstorm
```

Текущая версия программы:

```
CONTAINER_VERSION=0.16.0
```

Ключи доступа к хранилищу прикрепляемых файлов в TeamStorm (minio):

```
AWS_ACCESS_KEY=testitAccessKey
AWS_SECRET_KEY=testitSecretKey
```

Параметры подключения к RabbitMQ:

```
RABBITMQ_DEFAULT_HOST=teamstorm_rabbitmq
RABBITMQ_DEFAULT_PASS=password
RABBITMQ_DEFAULT_USER=teamstorm
RABBITMQ_DEFAULT_VHOST=teamstorm
```

Параметры подключения к БД, при установке внешней БД, поменять на свои значения:

```
POSTGRES_ATTACHMENT_DB=teamstorm_attachment_db
POSTGRES_COMMENT_DB=teamstorm_comment_db
POSTGRES_WORKFLOW_DB=teamstorm_workflow_db
POSTGRES_WORKITEM_LINK_RULE_DB=teamstorm_linkrule_db
POSTGRES_DB=teamstormdb
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
```

Для каждой конкретной базы данных значения по умолчанию можно поменять в строке подключения к базе данных:

```
PG_CONNECTION_STRING="Host=postgres;Port=5432;Database=${POSTGRES_DB};Username=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};Pooling=true"
PG_ATTACHMENT_CONNECTION_STRING="Host=attachmnet_service_postgres;Port=5432;Database=${POSTGRES_ATTACHMENT_DB};Username=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};"
PG_COMMENT_CONNECTION_STRING="Host=comment_service_postgres;Port=5432;Database=${POSTGRES_COMMENT_DB};Username=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};"
PG_WORKFLOW_CONNECTION_STRING="Host=workflow_service_postgres;Port=5432;Database=${POSTGRES_WORKFLOW_DB};Username=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};"
PG_WORKITEM_LINK_RULE_CONNECTION_STRING="Host=workitem_link_rule_service_postgres;Port=5432;Database=${POSTGRES_WORKITEM_LINK_RULE_DB};Username=${POSTGRES_USER};Password=${POSTGRES_PASSWORD};"
Pooling=true
```

Системные параметры, оставить без изменений:

```
ASPNETCORE_ENVIRONMENT=Production
FILE_BUCKET_NAME=teamstorm 
```
