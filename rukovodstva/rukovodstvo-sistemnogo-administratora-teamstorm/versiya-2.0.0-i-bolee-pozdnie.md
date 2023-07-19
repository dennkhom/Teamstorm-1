# Версия 2.0.0 и более поздние

## Назначение документа

Документ описывает действия системного администратора по установке и настройке ПО TeamStorm v. 2.0.0 и выше.

## **Установка программного ПО**

Установка ПО TeamStorm осуществляется только после предварительной установки [ПО Test IT](https://testit.software/versions/).

Установка Test IT описана в [документации Test IT](https://docs.testit.software/installation-guide/).

ПО TeamStorm необходимо устанавливать на тот же хост, на который установлено ПО Test IT.

### Установка, перезапуск и удаление в Docker Compose

#### **Требования**

​[Docker Engine 20.10.17 и выше](https://docs.docker.com/engine).

[Docker Compose 2.10.0 и выше.](https://docs.docker.com/compose)

[Test IT 4.2.4 и выше, рекомендуется последняя версия](https://testit.software/versions/).

{% hint style="info" %}
Для установки версии 2.0.0 необходимо предварительно обновить TeamStorm до версии 1.34.0
{% endhint %}

#### **Состав поставки**

`images.tar` - архив с образами (только в архиве для автономной установки)**.**

Состав поставки TeamStorm:

* `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам TeamStorm;
* `docker-compose.yml` - конфигурационный файл Docker Compose.

#### **Установка и настройка Test IT**

1. Загрузите и установите ПО Test IT в соответствии с документацией Test IT.
2.  Настройте поддержку TeamStorm в Test IT, заменив значение переменной `CWM_ENABLED`:

    ```shell
    # Отредактируйте файл переменных окружения Test IT:
    vi ./testit/.env
    <<<
    CWM_ENABLED="false"
    >>>
    CWM_ENABLED="true"
    ```
3.  Настройте поддержку WIKI в Test IT установив значения переменных `WIKI_ENABLED` и `WIKI_S3_BUCKET_SECRET_KEY`:

    ```shell
    # Отредактируйте файл переменных окружения Testit:
    vi ./testit/.env
    <<<
    WIKI_ENABLED="false"
    >>>
    WIKI_ENABLED='true'
    ```
4.  Следующие переменные в конфигурационных файлах `testit` и `teamstorm` должны совпадать:

    | `testit/.env`                 |        `teamstorm/.env`       | Комментарий                                                                  |
    | ----------------------------- | :---------------------------: | ---------------------------------------------------------------------------- |
    | FRONTEND\_URL                 |       CWM\_FRONTEND\_URL      | Например, "[https://teamstorm.mycompany.ru](https://teamstorm.mycompany.ru)" |
    | CWM\_S3\_BUCKET\_SECRET\_KEY  |  CWM\_S3\_BUCKET\_SECRET\_KEY | Переменная не должна содержать символ `$`                                    |
    | WIKI\_S3\_BUCKET\_SECRET\_KEY | WIKI\_S3\_BUCKET\_SECRET\_KEY | Переменная не должна содержать символ `$`                                    |
5.  Убедитесь, что сервис `auth`  имеет настройку для редактирования ролей:

    ```shell
    $ vi ./testit/docker-compose.yml
    ...
    auth:
    ...
        environment:
    >>>   CAN_EDIT_SYSTEM_ROLES: "${CAN_EDIT_SYSTEM_ROLES:-true}"
    ```

При обновлении с Test IT 4.2.4 на Test IT 4.3.0 дополнительные действия не требуются.

#### **Подготовка**

1. Измените значения переменных по умолчанию в .env-файле.
2.  Задайте параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:

    ```bash
    echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
    sysctl -p
    ```
3. Заблокируйте все порты, кроме порта 80, необходимого для доступа к пользовательскому интерфейсу.
4.  **Опционально:** для обслуживания системы посредством протокола SSH необходимо открыть порт 22 (может быть переназначено на конкретной конфигурации). Для работы по HTTPS необходимо открыть порт 443. Пример открытия доступа к портам для CentOS 8:

    ```bash
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    firewall-cmd --zone=public --add-port=22/tcp --permanent
    firewall-cmd --zone=public --add-port=443/tcp --permanent
    firewall-cmd --reload
    ```

#### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v2.0.0`.
2. Создайте сеть и кластер вручную или воспользуйтесь скриптом автоматического развертывания из поставки:

```bash
cd ${PROJECT_HOME}/teamstorm
docker network create yoonion_network
docker compose -p teamstorm -f docker-compose.yml up -d
cd ${PROJECT_HOME}/testit
docker compose -p testit -d docker-compose.yml up -d
```

```bash
tar -xzvf teamstorm_v2.0.0.tgz
cd ${PROJECT_HOME}/teamstorm
chmod +x setup.sh
./setup_teamstorm.sh
```

#### **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

```bash
cd ${PROJECT_HOME}/teamstorm
docker-compose -f docker-compose.yml -p teamstorm restart
```

#### Удаление системы

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```bash
cd teamstorm_v2.0.0
docker-compose -f docker-compose.yml --project-name teamstorm down --volumes --timeout 120
```

Чтобы сохранить информацию для последующего использования, выполните команду без флага `--volumes`.

## Описание .env файла

Репозиторий для скачивания образов установки TeamStorm:

```shell
CWM_DOCKER_REGISTRY="docker.testit.ru/teamstorm"
```

Текущая версия программы:

```shell
CWM_CONTAINER_VERSION="2.0.0"
```

Ключи доступа к хранилищу прикрепляемых файлов в TeamStorm (minio):

```shell
AWS_ACCESS_KEY="testitAccessKey"
AWS_SECRET_KEY="testitSecretKey"
```

Параметры подключения к RabbitMQ:

```shell
RABBITMQ_DEFAULT_HOST="teamstorm_rabbitmq"
RABBITMQ_DEFAULT_PASS="password"
RABBITMQ_DEFAULT_USER="teamstorm"
RABBITMQ_DEFAULT_VHOST="teamstorm"
```

Параметры подключения к базе данных (при установке внешней базы данных поменять на свои значения):

```shell
POSTGRES_DEFAULT_DB="postgres"
POSTGRES_DEFAULT_USER="postgres"
POSTGRES_DEFAULT_PASSWORD="password"
POSTGRES_DEFAULT_PORT="5432"
POSTGRES_DEFAULT_HOST="database_service"
```

Для каждой конкретной базы данных значения по умолчанию можно заменить:

```shell
POSTGRES_ATTACHMENT_DB_HOST="${POSTGRES_DEFAULT_HOST}"
POSTGRES_ATTACHMENT_DB_DB="teamstorm_attachment_db"
POSTGRES_ATTACHMENT_DB_USER="${POSTGRES_DEFAULT_USER}"
POSTGRES_ATTACHMENT_DB_PASSWORD="${POSTGRES_DEFAULT_PASSWORD}"
POSTGRES_ATTACHMENT_DB_PORT="${POSTGRES_DEFAULT_PORT}"
POSTGRES_COMMENT_DB_HOST="${POSTGRES_DEFAULT_HOST}"
POSTGRES_COMMENT_DB_DB="teamstorm_comment_db"
POSTGRES_COMMENT_DB_USER="${POSTGRES_DEFAULT_USER}"
POSTGRES_COMMENT_DB_PASSWORD="${POSTGRES_DEFAULT_PASSWORD}"
POSTGRES_COMMENT_DB_PORT="${POSTGRES_DEFAULT_PORT}"
POSTGRES_DB_HOST="${POSTGRES_DEFAULT_HOST}"
```

Из вышеуказанных значений формируется строка подключения к базе данных:

```shell
PG_ATTACHMENT_CONNECTION_STRING="Host=${POSTGRES_DEFAULT_HOST};Port=${POSTGRES_DEFAULT_PORT};Database=teamstorm_attachment_db;Username=${POSTGRES_ATTACHMENT_DB_USER};Password=${POSTGRES_ATTACHMENT_DB_PASSWORD};"
PG_COMMENT_CONNECTION_STRING="Host=${POSTGRES_DEFAULT_HOST};Port=${POSTGRES_DEFAULT_PORT};Database=teamstorm_comment_db;Username=${POSTGRES_COMMENT_DB_USER};Password=${POSTGRES_COMMENT_DB_PASSWORD};"
PG_CONNECTION_STRING="Host=${POSTGRES_DEFAULT_HOST};Port=${POSTGRES_DEFAULT_PORT};Database=teamstormdb;Username=${POSTGRES_DB_USER};Password=${POSTGRES_DB_PASSWORD};Pooling=true"

```

Настройте параметры почтового сервера для уведомлений:

`CWMFRONTENDURL="${CWM_FRONTEND_URL}"` — указать хост размещения TeamStorm (при необходимости);

`MAILSERVERSETTINGS__HOST="${MAIL_SERVER_HOST}"`  — указать хост почтового сервера. Например, mail.outlook.com;

`MAILSERVERSETTINGS__PORT="${MAIL_SERVER_PORT}"`  — указать порт почтового сервера. Например, 587;

`MAILSERVERSETTINGS__FROM="${MAIL_SERVER_FROM}"` — указать сервисный аккаунт от имени которого будет происходить нотификация. Например, service@emailserver.com;

`MAILSERVERSETTINGS__DISPLAYNAME=`"`${MAIL_SERVER_DISPLAY_NAME}"` — указать имя сервисного аккаунта для отображения . Например, Mail Service;

`MAILSERVERSETTINGS__USESTARTTLS="${MAIL_SERVER_USE_START_TLS}"` — использовать TLS для подключения к почстовому серверу;

`MAILSERVERSETTINGS__USESSL="${MAIL_SERVER_USE_SSL}"` — использовать SSL для подключения к почтовому серверу;&#x20;

`MAIL_SERVER_TZ`=`"Europe/Moscow"` — указать идентификатор часового пояса (по умолчанию установлено московское время). Таблица идентификаторов находится по адресу [https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List)

Системные параметры оставить без изменений:

```shell
ASPNETCORE_ENVIRONMENT="Production"
FILE_BUCKET_NAME="teamstorm"
```
