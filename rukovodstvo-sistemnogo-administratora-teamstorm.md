# Руководство системного администратора TeamStorm

## Назначение документа

Документ описывает действия системного администратора по установке, настройке, обновлению и резевному копированию ПО TeamStorm.

## Установка, перезапуск и удаление в Docker Compose

### **Установка программного ПО**

#### **Требования**

​[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)​

Docker Engine 20.10.17 и выше

Docker Compose 1.17.0 и выше

#### **Состав поставки**

images.tar - архив с образами (только в архиве для автономной установки)**.** Установка TeamStorm выполняется вместе с установкой TestIT.

Состав поставки TestIT указан в [документации на ПО TestIT](https://docs.testit.software/installation-guide/ustanovka-perezapusk-i-udalenie-v-docker-compose#sostav-postavki).

* `setup.sh` - основной скрипт установки;
* `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам Teamstorm;
* `docker-compose.yml` - конфигурационный файл Docker Compose.

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
5. **Опционально**: включите логирование пользовательских действий как описано ниже. По умолчанию оно отключено.

#### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v0.16.0.`

Выполните следующие команды:

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

#### **Полная очистка данных**

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```bash
cd teamstorm_v0.16.0
docker-compose -f docker-compose.yml --project-name teamstorm down --volumes --timeout 120
```

Чтобы сохранить информацию для последующего использования, выполните команду без флага `--volumes`.

### Описание .env файла

Репозиторий для скачивания образов установки Teamstorm:

`DOCKER_REGISTRY=docker.testit.ru/teamstorm`

Текущая версия программы:

`CONTAINER_VERSION=0.16.0`

Адрес TeamStorm используется в качестве обратной ссылки. Вам необходимо задать эту переменную, если вы разворачиваете Frontend и Backend на разных серверах

`FRONTEND_URL=http://localhost`

Сертификат для настройки HTTPS, ключ для настройки HTTPS, true - редирект HTTP на HTTPS:

```
## internal certificate path
# SSL_CERTIFICATE=/etc/nginx/ssl/testit.crt
# SSL_CERTIFICATE_KEY=/etc/nginx/ssl/testit.key
# REDIRECT_TO_HTTPS=true
```

Принудительное отключение проверки сертификата для внешнего сервиса через ";" можно указывать несколько сервисов:

`# INSECURE_REMOTES=example.com:443`

Ключи доступа к хранилищу прикрепляемых файлов в TeamStorm (minio):

```
AWS_ACCESS_KEY=testitAccessKey
AWS_SECRET_KEY=testitSecretKey
```

Ключи доступа к хранилищу "avatars" в TeamStorm (minio):

```
AVATARS_AWS_ACCESS_KEY=avatarsAccessKey
AVATARS_AWS_SECRET_KEY=avatarsSecretKey
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
ASPNETCORE_ACCESS_TOKEN_EXPIRATION_MINUTES=8000
ASPNETCORE_REFRESH_TOKEN_EXPIRATION_MINUTES=88000
FILE_BUCKET_NAME=testit
```

Минимальный пул рабочих потоков на процессор. Используется для WebAPI. Чем выше значение, тем большее количество пользователей будут одновременно обслуживаться, что равномерно повысит нагрузку:

`THREAD_PER_PROCESSOR=10`

"false" - если OpenId провайдер не поддерживает PKCE:

`USE_PKCE=true`

Необходимый параметр, если внешний сервис Jira заблокирован для исходящих подключений. Указанное значение в секундах, время через которое TeamStorm инициирует входящее подключение к Jira для синхронизации данных:

`SYNC_RESULT_LINKS_EVERY_SEC=120`

Если вы меняли порты по умолчанию для контейнера webAPI, то нужно изменить следующий параметр:

`WEBAPI_URL=http://webapi`

### Настройка внешних подключений

#### Настройка внешнего подключения RabbitMQ

Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.

1.  Укажите в файле `.env` для следующих параметров значения, установленные вами при настройке RabbitMQ (ниже указаны значения по умолчанию):

    ```
    RABBITMQ_DEFAULT_USER=testit
    RABBITMQ_DEFAULT_PASS=password
    RABBITMQ_DEFAULT_VHOST=testitrabbit
    RABBITMQ_DEFAULT_HOST=external-server (где external-server - ip-адрес или DNS-имя вашего сервера с RabbitMQ)
    RABBITMQ_DEFAULT_PORT=5672
    RABBITMQ_AUTH_MODE=plain
    RABBITMQ_CLIENT_CERT_PATH=/etc/rabbitmq/ssl/client/testit.pfx
    #RABBITMQ_CLIENT_CERT_PASSPHRASE=
    ```
2. В файле `docker-compose.yml` закомментируйте секцию с сервисом rabbitmq зависимости от него других контейнеров (все упоминания rabbitmq в блоках `depends_on`) и `rabbit-volume`, `rabbitmq-configuration-volume`, `rabbitmq-certificates-volume` в списке `volumes`.
3.  Перезапустите систему:

    `docker-compose -f docker-compose.yml --project-name teamstorm up --detach --timeout 120 --remove-orphans`



#### Настройка внешнего подключения minio и avatars.minio

Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.

Вы можете использовать одну базу данных для сервисов minio и avatars.minio.

1. Создайте два бакета. Например, bucket1 на замену сервису minio и bucket2 на замену сервису avatars.minio.
2. Для каждого из бакетов создайте пару Access Key и Secret Key.
3.  В .env файле:

    * Для сервиса minio установите следующие значения переменных:

    ```
    AWS_ACCESS_KEY: (Access key, установленный для bucket1)
    AWS_SECRET_KEY: (Secret Key, установленный для bucket1)
    AWS_CONNECTION_STRING: http://external-server:9000 (где external-server - ip-адрес или DNS-имя вашего сервера с minio)
    FILE_BUCKET_NAME: (в данном примере, bucket1)
    ```

    * Для сервиса avatars.minio установите следующие значения переменных:

    ```
    AVATARS_AWS_ACCESS_KEY: (Access key, установленный для bucket2)
    AVATARS_AWS_SECRET_KEY: (Secret Key, установленный для bucket2)
    AVATARS_AWS_CONNECTION_STRING: http://external-server:9000
    ```
4.  Добавьте для сервиса avatars.minio следующую переменную:

    `AVATARS_FILE_BUCKET_NAME: (в данном примере, bucket2)`
5.  Добавьте в файл `docker-compose.yml` в секции `avatars.api` в разделе `environment` следующую строку:

    `AWSS3Server__FileBucketName: “${AVATARS_FILE_BUCKET_NAME}”`
6. В файле `docker-compose.yml` закомментируйте секцию с сервисами minio и avatars.minio, зависимости от них других контейнеров (все упоминания сервисов в блоках `depends_on`), и их вольюмы (`minio-export-volume` и `minio-data-volume`) в списке volumes.
7.  Перезапустите систему:

    `docker-compose -f docker-compose.yml --project-name teamstorm up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения базы данных Redis

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

1. При настройке внешней базы данных Redis установите параметр `appendonly yes`
2. В файле `docker-compose.yml` закомментируйте или удалите секцию с сервисом БД (auth\_cache), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках `depends_on`), и его вольюм (`auth-cache-volume`) в списке `volumes` в файле `docker-compose.yml`.
3. В `.env` файле укажите данные для подключения к внешней БД, где external-server - IP или DNS-имя хоста, на котором установлен Redis (без указания протокола и порта): `AUTH_CACHE_CONNECTION_STRING=external-server`
4.  Перезапустите систему:

    `docker-compose -f docker-compose.yml --project-name teamstorm up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения базы данных InfluxDB

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

1.  При настройке внешней базы данных InfluxDB установите следующие параметры:

    ```
    max-series-per-database = 0
    max-values-per-tag = 0
    ```
2. Закомментируйте или удалите секцию с сервисом БД (influxdb), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках `depends_on`), и его вольюм (`influx-volume`) в списке volumes в файле `docker-compose.yml`.
3.  В .env файле укажите данные для подключения к внешней БД, где external-server - IP-адрес или DNS-имя вашего сервера с InfluxDB.

    `INFLUX_CONNECTION_STRING=http://external-server:8086`
4.  Перезапустите систему:

    `docker-compose -f docker-compose.yml --project-name teamstorm up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения базы данных PostgreSQL

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

1.  Подготовьте внешнюю БД (PostgreSQL) для каждого сервиса (имена баз данных: testitdb, authdb, avatarsdb):

    ```
    yum install postgresql-contrib
    psql -U postgres
    create database testitdb;
    create user tester with encrypted password 'tester';
    grant all privileges on database testitdb to tester;
    \connect testitdb;
    CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;
    ```
2.  Для остальных БД (authdb, avatarsdb) нужно выполнить скрипт, приведенный ниже, подставив название БД вместо testitdb:

    ```
    create database authdb;
    grant all privileges on database authdb to tester;
    \connect authdb;
    CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;
    create database avatarsdb;
    grant all privileges on database avatarsdb to tester;
    \connect avatarsdb;
    CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;
    ```
3. Закомментируйте или удалите секцию с сервисом БД (authdb, avatars.db, db), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках `depends_on`), и его вольюмы (`authdb-volume`, `db-volume`, `avatars.db-volume`) в списке volumes в файле `docker-compose.yml`.
4.  В `.env` файле укажите данные для подключения к внешней БД. (ip-external server/dns-name, port, login, password). В Host можно указать отдельные СУБД или одну и ту же СУБД для каждой БД.

    ```
    DB_CONNECTION_STRING=Host=external_server1;Port=5432;Database=testitdb;Username=tester;Password=tester;Pooling=true;Maximum Pool Size=130
    #POSTGRES_DB=testitdb
    #POSTGRES_USER=postgres
    #POSTGRES_PASSWORD=password
    ...
    AUTH_CONNECTION_STRING=Host=external_server2;Port=5432;Database=authdb;Username=tester;Password=tester;Pooling=true;Maximum Pool Size=130
    #POSTGRES_AUTH_DB=authdb
    #POSTGRES_AUTH_USER=postgres
    #POSTGRES_AUTH_PASSWORD=password
    ...
    AVATARS_CONNECTION_STRING=Host=external_server3;Port=5432;Database=avatarsdb;Username=tester;Password=tester
    #POSTGRES_AVATARS_DB=avatarsdb
    #POSTGRES_AVATARS_USER=postgres
    #POSTGRES_AVATARS_PASSWORD=password
    ```
5.  Выполните установку TeamStorm:

    `docker-compose -f docker-compose.yml --project-name teamstorm up --detach --timeout 120`

## Обновление ПО

Обновление TeamStorm осуществляется с помощью установки новой версии.

## Настройка HTTPS

Перед настройкой https необходимо провести базовую настройку и установку TeamStorm.

1. Откройте 443 порт, подробности смотрите в инструкции по установке.
2. Перейти в каталог с папкой TestIT `cd testit_v3.5.3`
3. В `.env` файле системы TestIT раскомментируйте переменные `SSL_CERTIFICATE` и `SSL_CERTIFICATE_KEY`.

```
## internal certificate path
SSL_CERTIFICATE=/etc/nginx/ssl/testit.crt
SSL_CERTIFICATE_KEY=/etc/nginx/ssl/testit.key
#REDIRECT_TO_HTTPS=true
```

Раскомментируйте R`EDIRECT_TO_HTTPS` для редиректа http на https.

Чтобы сервис frontend был доступен по https протоколу, пропишите 443 порт в файле `docker-compose.yml`:

```
services:
frontend:
image: "${DOCKER_REGISTRY}/frontend:${CONTAINER_VERSION}"
ports:
- 80:80
- 443:443
...
```

Подготовьте файлы с сертификатом и ключом. Для этого дайте им имена `testit.crt` и `testit.key`.

Имена файлов сертификатов должны соответствовать значению переменных `SSL_CERTIFICATE` и `SSL_CERTIFICATE_KEY` в `.env` файле.

Cкопируйте подготовленные файлы в хранилище сертификатов:

```
certs=$(docker inspect testit_ssl-volume --format '{{ .Mountpoint }}')
cp testit.crt ${certs}/
cp testit.key ${certs}/
```

Примените изменения, выполнив команду:

`docker-compose -f docker-compose.yml --project-name testit up --detach --timeout 120`
