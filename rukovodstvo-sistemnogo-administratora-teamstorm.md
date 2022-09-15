# Руководство системного администратора TeamStorm

## Назначение документа

Документ описывает действия системного администратора по установке, настройке, обновлению и резевному копированию ПО TeamStorm.

## Установка, перезапуск и удаление в Docker Compose

### **Установка программного ПО**

#### **Требования**

​[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)​

Docker Engine 17.09.0 и выше

Docker Compose 1.17.0 и выше

#### **Состав поставки**

* .env - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам Teamstorm
* docker-compose.yml - конфигурационный файл Docker Compose
* docker-compose.elk.yml - конфигурационный файл Docker Compose с базами данных Elasticsearch, Logstash и Kibana
* backup.sh - скрипт запуска резервного копирования
* restore.sh - скрипт восстановления из резервной копии
* images.tar.gz - архив с образами (только в архиве для автономной установки)

#### **Подготовка**

1. Измените дефолтные значения переменных в .env-файле.
2. Задайте параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:  

    ```
    echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
    sysctl -p
    ```
3. Заблокируйте все порты, кроме порта 80, необходимого для доступа к пользовательскому интерфейсу.
4. **Опционально:** для обслуживания системы посредством протокола SSH, необходимо открыть порт 22 (может быть переназначено на конкретной конфигурации). Для работы по HTTPS необходимо открыть порт 443. Пример открытия доступа к портам для CentOS 7:  

    ```
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    firewall-cmd --zone=public --add-port=22/tcp --permanent
    firewall-cmd --zone=public --add-port=443/tcp --permanent
    firewall-cmd --reload
    ```
5. **Опционально**: включите логирование пользовательских действий как описано ниже. По умолчанию оно отключено.

#### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker образы с публичных репозиториев. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm`.

Если вы используете версию Docker Compose 1.20.0 и выше, выполните следующие команды:

```
cd ~/teamstorm
docker load -i images.tar.gz
docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120
```

Если вы используете версию Docker Compose 1.17.0-1.19.0, выполните следующие команды:

```
cd ~/teamstorm
docker load -i images.tar.gz
docker-compose -f docker-compose.yml --project-name prod up -d
```

#### **Online-установка**

Скачайте файлы online-установки. Распакуйте содержимое архива online-установки, например, в папку `~/teamstorm`. Если вы используете версию Docker Compose 1.20.0 и выше, выполните следующие команды:

```
cd ~/teamstorm
docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120
Если вы используете версию Docker Compose 1.17.0-1.19.0, выполните следующие команды:
cd ~/teamstorm
docker-compose -f docker-compose.yml --project-name prod up -d
```

#### **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

```
docker-compose -f docker-compose.yml --project-name prod restart --timeout 120
```

#### **Полная очистка данных**

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```
docker-compose -f docker-compose.yml --project-name prod down --volumes --timeout 120
```

Чтобы сохранить информацию  для последующего использования, выполните команду без флага `--volumes`.

### Описание .env файла

Репозиторий для скачивания образов установки Teamstorm:

`DOCKER_REGISTRY=registry.testit.software/teamstorm`

Текущая версия программы:

`CONTAINER_VERSION=3.0.0`

Адрес TeamStorm используется в качестве обратной ссылки. Вам необходимо задать эту переменную, если вы разворачиваете Frontend и Backend на разных серверах

`FRONTEND_URL=http://localhost`

Сертификат для настройки HTTPS, ключ для настройки HTTPS, true - редирект HTTP на HTTPS:

```
## internal certificate path
#SSL_CERTIFICATE=/etc/nginx/ssl/testit.crt
#SSL_CERTIFICATE_KEY=/etc/nginx/ssl/testit.key
#REDIRECT_TO_HTTPS=true
```

Принудительное отключение проверки сертификата для внешнего сервиса через ";" можно указывать несколько сервисов:

`#INSECURE_REMOTES=example.com:443`

Ключи доступа к хранилищу прикрепляемых файлов в TeamStorm (minio):

```
AWS_ACCESS_KEY=testitAccessKey
AWS_SECRET_KEY=testitSecretKey
```

Ключи доступа к хранилищу "avatars" в Test IT (minio):

```
AVATARS_AWS_ACCESS_KEY=avatarsAccessKey
AVATARS_AWS_SECRET_KEY=avatarsSecretKey
```

Параметры подключения к RabbitMQ:

```
RABBITMQ_DEFAULT_USER=testit
RABBITMQ_DEFAULT_PASS=F1rstL0g0N!
RABBITMQ_DEFAULT_VHOST=testitrabbit
RABBITMQ_DEFAULT_HOST=rabbitmq
RABBITMQ_DEFAULT_PORT=5672
RABBITMQ_AUTH_MODE=plain
RABBITMQ_CLIENT_CERT_PATH=/etc/rabbitmq/ssl/client/testit.pfx
#RABBITMQ_CLIENT_CERT_PASSPHRASE=
```

Параметры подключения к БД, при установке внешней БД, поменять на свои значения:

```
DB_CONNECTION_STRING=Host=db;Port=5432;Database=testitdb;Username=postgres;Password=F1rstL0g0N!;Pooling=true;Maximum Pool Size=130
```

Данные для создания базы данных, пользователя и пароля в  поставке по умолчанию:

```
DB_CONNECTION_STRING=Host=db;Port=5432;Database=testitdb;Username=postgres;Password=F1rstL0g0N!;Pooling=true;Maximum Pool Size=130
POSTGRES_DB=testitdb
POSTGRES_USER=postgres
POSTGRES_PASSWORD=F1rstL0g0N!
```

Аналогично с Auth DB:

```
AUTH_CONNECTION_STRING=Host=authdb;Port=5432;Database=authdb;Username=postgres;Password=F1rstL0g0N!;Pooling=true;Maximum Pool Size=130
POSTGRES_AUTH_DB=authdb
POSTGRES_AUTH_USER=postgres
POSTGRES_AUTH_PASSWORD=F1rstL0g0N!
```

Аналогично с Avatar DB:

```
AVATARS_CONNECTION_STRING=Host=avatars-db;Port=5432;Database=avatarsdb;Username=postgres;Password=F1rstL0g0N!
POSTGRES_AVATARS_DB=avatarsdb
POSTGRES_AVATARS_USER=postgres
POSTGRES_AVATARS_PASSWORD=F1rstL0g0N!
```

Системные параметры, оставить без изменений:

```
ASPNETCORE_ENVIRONMENT=Production
ASPNETCORE_ACCESS_TOKEN_EXPIRATION_MINUTES=8000
ASPNETCORE_REFRESH_TOKEN_EXPIRATION_MINUTES=88000
FILE_BUCKET_NAME=testit
```

Уровень логирования. Можно изменить Warning на Information для более детального логирования, что повысит нагрузку на систему:

`API_LOG_LEVEL=Warning`

Минимальный пул рабочих потоков на процессор. Используется для WebAPI. Чем выше значение, тем большее количество пользователей будут одновременно обслуживаться, что равномерно повысит нагрузку:

`THREAD_PER_PROCESSOR=10`

"false" - если OpenId провайдер не поддерживает PKCE:

`USE_PKCE=true`

Необходимый параметр, если внешний сервис Jira заблокирован для исходящих подключений. Указанное значение в секундах, время через которое TeamStorm инициирует входящее подключение к Jira для синхронизации данных:

`SYNC_RESULT_LINKS_EVERY_SEC=120`

Период хранения бизнес логов по действиям пользователей в Elasticsearch, максимальный объем хранения логов (GB), имя индексов логов в elasticsearch:

```
EVENT_LOG_MAX_AGE=30d
EVENT_LOG_MAX_SIZE=50gb
ELASTICSEARCH_LOGS_INDEX=action_logs
```

Если вы меняли порты по умолчанию для контейнера webAPIто нужно изменить следующий параметр:

`WEBAPI_URL=http://webapi`

### Настройка внешних подключений

#### Настройка внешнего подключения RabbitMQ

Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.

1. Укажите в файле .env для следующих параметров значения, установленные вами при настройке RabbitMQ (ниже указаны значения по умолчанию):  

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
3. Перезапустите систему:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения стека Logstash и Kibana (ELK)

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

1. При настройке стека ELK укажите в .env файле следующие параметры соответственно вашей конфигурации:  

    ```
    ELASTICSEARCH_CONNECTION_STRING=http://external-server:9200 (где external-server - ip-адрес или DNS-имя вашего сервера с Elasticsearch)
    ELASTICSEARCH_INDEX= (заданное вами имя индекса для TeamStorm)
    ELASTICSEARCH_LOGS_INDEX= (заданное вами имя индекса логов)
    ```
    
2. В файле `docker-compose.yml` закомментируйте секции с сервисами Elasticsearch, Logstash, Kibana зависимости от него других контейнеров (все упоминания elasticsearch, logstash, kibana в блоках depends\_on) и elastic-volume в списке volumes.
3. Перезапустите систему:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения minio и avatars.minio

Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.

Вы можете использовать одну базу данных для сервисов minio и avatars.minio.

1. Создайте два бакета. Например, bucket1 на замену сервису minio и bucket2 на замену сервису avatars.minio.
2. Для каждого из бакетов создайте пару Access Key и Secret Key.
3. В .env файле:  

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

1. Добавьте для сервиса avatars.minio следующую переменную: 

    `AVATARS_FILE_BUCKET_NAME: (в данном примере, bucket2)`

1. Добавьте в файл `docker-compose.yml` в секции `avatars.api` в разделе `environment` следующую строку:  

    `AWSS3Server__FileBucketName: “${AVATARS_FILE_BUCKET_NAME}”`

1. В файле `docker-compose.yml` закомментируйте секцию с сервисами minio и avatars.minio, зависимости от них других контейнеров (все упоминания сервисов в блоках `depends_on`), и их вольюмы (`minio-export-volume` и `minio-data-volume`) в списке volumes.
2. Перезапустите систему:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения базы данных Redis

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

1. При настройке внешней базы данных Redis установите параметр `appendonly yes`
1. В файле `docker-compose.yml` закомментируйте или удалите секцию с сервисом БД (auth\_cache), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках `depends_on`), и его вольюм (`auth-cache-volume`) в списке `volumes` в файле `docker-compose.yml`.
2. В `.env` файле укажите данные для подключения к внешней БД, где external-server - IP или DNS-имя хоста, на котором установлен Redis (без указания протокола и порта): `AUTH_CACHE_CONNECTION_STRING=external-server`
4. Перезапустите систему:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения базы данных InfluxDB

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

1. При настройке внешней базы данных InfluxDB установите следующие параметры:

```
max-series-per-database = 0
max-values-per-tag = 0
```

1. Закомментируйте или удалите секцию с сервисом БД (influxdb), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках `depends_on`), и его вольюм (`influx-volume`) в списке volumes в файле `docker-compose.yml`.
2. В .env файле укажите данные для подключения к внешней БД, где external-server - IP-адрес или DNS-имя вашего сервера с InfluxDB.

`INFLUX_CONNECTION_STRING=http://external-server:8086`

1. Перезапустите систему:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans`

#### Настройка внешнего подключения базы данных PostgreSQL

{% hint style="info" %}
Версия внешнего сервиса должна совпадать с версией, указанной в файле `docker-compose.yml`.
{% endhint %}

Подготовьте внешнюю БД (PostgreSQL) для каждого сервиса (имена баз данных: testitdb, authdb, avatarsdb):

```
yum install postgresql-contrib
psql -U postgres
create database testitdb;
create user tester with encrypted password 'tester';
grant all privileges on database testitdb to tester;
\connect testitdb;
CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;
```

Для остальных БД (authdb, avatarsdb) нужно выполнить скрипт ниже подставив название БД вместо testitdb:

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

Закомментируйте или удалите секцию с сервисом БД (authdb, avatars.db, db), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках `depends_on`), и его вольюмы (`authdb-volume`, `db-volume`, `avatars.db-volume`) в списке volumes в файле `docker-compose.yml`.

В `.env` файле укажите данные для подключения к внешней БД. (ip-external server/dns-name, port, login, password). В Host можно указать отдельные СУБД или одну и ту же СУБД для каждой БД.

```
DB_CONNECTION_STRING=Host=external_server1;Port=5432;Database=testitdb;Username=tester;Password=tester;Pooling=true;Maximum Pool Size=130
#POSTGRES_DB=testitdb
#POSTGRES_USER=postgres
#POSTGRES_PASSWORD=F1rstL0g0N!
...
AUTH_CONNECTION_STRING=Host=external_server2;Port=5432;Database=authdb;Username=tester;Password=tester;Pooling=true;Maximum Pool Size=130
#POSTGRES_AUTH_DB=authdb
#POSTGRES_AUTH_USER=postgres
#POSTGRES_AUTH_PASSWORD=F1rstL0g0N!
...
AVATARS_CONNECTION_STRING=Host=external_server3;Port=5432;Database=avatarsdb;Username=tester;Password=tester
#POSTGRES_AVATARS_DB=avatarsdb
#POSTGRES_AVATARS_USER=postgres
#POSTGRES_AVATARS_PASSWORD=F1rstL0g0N!
```

Выполните установку TeamStorm:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120`

## Обновление ПО

{% hint style="info" %}
Если в `.env` и `.yml` файлах используются ваши пользовательские значения, перенесите их в файлы `.env` и `.yml` новой версии TeamStorm. Все значения в файлах `.env` и `.yml` новых версий TeamStorm заменяются на дефолтные при обновлении.
{% endhint %}

### **Подготовка**

Перед обновлением рекомендуется создать резервную копию установленной системы (файлы `docker-compose.yml` и `backup.sh` должны находиться в одной директории):

```
# В папке с текущей версией
chmod +x backup.sh
./backup.sh docker-compose.yml prod
```

### **Online обновление**

{% hint style="info" %}
Чтобы перенести информацию из volumes со старой версии на новую, имена проектов обеих версий должны совпадать. В наших примерах проект называется "prod".
{% endhint %}

1. Создайте новую директорию, скачайте и распакуйте в ней файл для онлайн установки.
2. ​В командной строке перейдите в директорию с последней версией и выполните:

```
docker-compose -f docker-compose.yml --project-name prod up -d --remove-orphans
```

### **Автономное обновление**

{% hint style="info" %}
Чтобы перенести информацию из volumes со старой версии на новую, имена проектов обеих версий должны совпадать. В наших примерах проект называется "prod".
{% endhint %}

1. Создайте новую директорию, скачайте и распакуйте в ней файл для автономной установки.
2. В командной строке перейдите в директорию с последней версией, распакуйте архив для автономной установки и выполните

```
docker load -i images.tar.gz
docker-compose -f docker-compose.yml --project-name prod up -d --remove-orphans
```

## Резервное копирование

### **Создание резервных копий**

Продукт будет остановлен на время создания резервной копии. Не следует создавать резервные копии из под `sudo`.

Перед выполнением скрипта на создание резервной копии следует перейти в директорию, которая содержит docker-compose.yml файл с настройками текущей версии системы.

Для создания резервной копии необходимо выполнить:

```
chmod +x scripts/backup.sh
scripts/backup.sh docker-compose.yml prod
```

Система будет запущена после окончания процесса. В рабочей директории будет создан архив с резервной копией. Формат имени файла архива: `backup_{день}_{месяц}_{год}.tar`. Например, `backup_21_05_2019.tar`.

### **Восстановление из резервной копии**

Продукт будет остановлен на время восстановления из резервной копии.

Перед выполнением скрипта на восстановление из резервной копии следует перейти в директорию, которая содержит docker-compose.yml и .env файлы с настройками текущей версии системы.

Для восстановления из резервной копии необходимо выполнить:

```
chmod +x scripts/restore.sh
scripts/restore.sh docker-compose.yml prod backup_21_05_2019.tar
```

Система будет запущена после окончания процесса.

При переносе продукта на другой сервер следует предварительно установить TeamStorm на новом сервере с настройками по умолчанию и затем восстановить данные системы из резервной копии. Для наилучшей совместимости на новом сервере рекомендуется устанавливать TeamStorm той же версии, которая содержится в резервной копии, переносимой из исходного сервера.

​

## Логирование пользовательских действий

Необходимо выставить параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:

```
echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
sysctl -p
```

Для включения опции логирования пользовательских действий:

1. Замените `docker-compose.yml` на содержимое `docker-compose.elk.yml`:

```
cd ~/teamstorm
cp docker-compose.yml docker-compose.yml.bak # резервная копия
cp docker-compose.elk.yml docker-compose.yml
```

&#x20;2\. Выполните шаги из разделов "Установка, перезапуск и удаление в Docker Compose" или "Обновление ПО".

## Настройка HTTPS

Перед настройкой https необходимо провести базовую настройку и установку TeamStorm.

1. Откройте 443 порт, подробности смотрите в инструкции по установке.
2. В `.env` файле раскомментируйте переменные `SSL_CERTIFICATE` и `SSL_CERTIFICATE_KEY`.

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
certs=$(docker inspect prod_ssl-volume --format '{{ .Mountpoint }}')
cp testit.crt ${certs}/
cp testit.key ${certs}/
```

Примените изменения, выполнив команду:

`docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120`
