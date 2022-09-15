# Руководство системного администратора TeamStorm

## Назначение документа&#x20;

Документ описывает  действия  системного администратора по установке, настройке, обновлению и резевному копированию ПО TeamStorm.

## Установка, перезапуск и удаление в Docker Compose

### **Установка программного ПО**

#### **Требования**

​[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)​

Docker Engine 17.09.0 и выше

Docker Compose 1.17.0 и выше

#### **Состав поставки**

* .env - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам Test IT
* docker-compose.yml - конфигурационный файл Docker Compose
* docker-compose.elk.yml - конфигурационный файл Docker Compose с базами данных Elasticsearch, Logstash и Kibana
* backup.sh - скрипт запуска резервного копирования
* restore.sh - скрипт восстановления из резервной копии
* images.tar.gz - архив с образами (только в архиве для автономной установки)

#### **Подготовка**

1. Измените дефолтные значения переменных в .env-файле
2. Задайте параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:

     ```echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
   sysctl -p```
   
4. Заблокируйте все порты, кроме порта 80, необходимого для доступа к пользовательскому интерфейсу.

**Опционально:** для обслуживания системы посредством протокола SSH, необходимо открыть порт 22 (может быть переназначено на конкретной конфигурации). Для работы по HTTPS необходимо открыть порт 443. Пример открытия доступа к портам для CentOS 7:

firewall-cmd --zone=public --add-port=80/tcp --permanent

firewall-cmd --zone=public --add-port=22/tcp --permanent

firewall-cmd --zone=public --add-port=443/tcp --permanent

firewall-cmd --reload

**Опционально**: включите логирование пользовательских действий как описано ниже. По умолчанию оно отключено.

#### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker образы с публичных репозиториев. Распакуйте содержимое архива автономной установки, например, в папку \~/testit.

Если вы используете версию Docker Compose 1.20.0 и выше, выполните следующие команды:

cd \~/testit

docker load -i images.tar.gz

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120

Если вы используете версию Docker Compose 1.17.0-1.19.0, выполните следующие команды:

cd \~/testit

docker load -i images.tar.gz

docker-compose -f docker-compose.yml --project-name prod up -d

#### **Online-установка**

Скачайте файлы online-установки. Распакуйте содержимое архива online-установки, например, в папку \~/testit. Если вы используете версию Docker Compose 1.20.0 и выше, выполните следующие команды:

cd \~/testit

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120

Если вы используете версию Docker Compose 1.17.0-1.19.0, выполните следующие команды:

cd \~/testit

docker-compose -f docker-compose.yml --project-name prod up -d

#### **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

docker-compose -f docker-compose.yml --project-name prod restart --timeout 120

#### **Полная очистка данных**

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

docker-compose -f docker-compose.yml --project-name prod down --volumes --timeout 120

Чтобы сохранить информацию из Test IT для последующего использования, выполните команду без флага --volumes.

### Описание .env файла

Репозиторий для скачивания образов установки Test IT:

DOCKER\_REGISTRY=registry.testit.software/testit

Текущая версия программы:

CONTAINER\_VERSION=3.0.0

Адрес Test IT, используется в качестве обратной ссылки. Вам необходимо задать эту переменную, если вы разворачиваете Frontend и Backend на разных серверах

FRONTEND\_URL=http://localhost

Сертификат для настройки HTTPS, ключ для настройки HTTPS, true - редирект HTTP на HTTPS:

\## internal certificate path

\#SSL\_CERTIFICATE=/etc/nginx/ssl/testit.crt

\#SSL\_CERTIFICATE\_KEY=/etc/nginx/ssl/testit.key

\#REDIRECT\_TO\_HTTPS=true

Принудительное отключение проверки сертификата для внешнего сервиса через ";" можно указывать несколько сервисов:

\#INSECURE\_REMOTES=example.com:443

Ключи доступа к хранилищу прикрепляемых файлов в Test IT (minio):

AWS\_ACCESS\_KEY=testitAccessKey

AWS\_SECRET\_KEY=testitSecretKey

Ключи доступа к хранилищу "avatars" в Test IT (minio):

AVATARS\_AWS\_ACCESS\_KEY=avatarsAccessKey

AVATARS\_AWS\_SECRET\_KEY=avatarsSecretKey

Параметры подключения к RabbitMQ:

RABBITMQ\_DEFAULT\_USER=testit

RABBITMQ\_DEFAULT\_PASS=F1rstL0g0N!

RABBITMQ\_DEFAULT\_VHOST=testitrabbit

RABBITMQ\_DEFAULT\_HOST=rabbitmq

RABBITMQ\_DEFAULT\_PORT=5672

RABBITMQ\_AUTH\_MODE=plain

RABBITMQ\_CLIENT\_CERT\_PATH=/etc/rabbitmq/ssl/client/testit.pfx

\#RABBITMQ\_CLIENT\_CERT\_PASSPHRASE=

Параметры подключения к БД, при установке внешней БД, поменять на свои значения:

DB\_CONNECTION\_STRING=Host=db;Port=5432;Database=testitdb;Username=postgres;Password=F1rstL0g0N!;Pooling=true;Maximum Pool Size=130

Данные для создания БД, пользователя и пароля в дефолтной поставке:

POSTGRES\_DB=testitdb

POSTGRES\_USER=postgres

POSTGRES\_PASSWORD=F1rstL0g0N!

Аналогично с Auth DB:

AUTH\_CONNECTION\_STRING=Host=authdb;Port=5432;Database=authdb;Username=postgres;Password=F1rstL0g0N!;Pooling=true;Maximum Pool Size=130

POSTGRES\_AUTH\_DB=authdb

POSTGRES\_AUTH\_USER=postgres

POSTGRES\_AUTH\_PASSWORD=F1rstL0g0N!

Аналогично с Avatar DB:

AVATARS\_CONNECTION\_STRING=Host=avatars-db;Port=5432;Database=avatarsdb;Username=postgres;Password=F1rstL0g0N!

POSTGRES\_AVATARS\_DB=avatarsdb

POSTGRES\_AVATARS\_USER=postgres

POSTGRES\_AVATARS\_PASSWORD=F1rstL0g0N!

Системные параметры, оставить без изменений:

ASPNETCORE\_ENVIRONMENT=Production

ASPNETCORE\_ACCESS\_TOKEN\_EXPIRATION\_MINUTES=8000

ASPNETCORE\_REFRESH\_TOKEN\_EXPIRATION\_MINUTES=88000

FILE\_BUCKET\_NAME=testit

Уровень логирования. Можно изменить Warning на Information для более детального логирования, что повысит нагрузку на систему:

API\_LOG\_LEVEL=Warning

Минимальный пул рабочих потоков на процессор. Используется для Webapi. Чем выше значение, тем большее количество пользователей будут одновременно обслуживаться, что равномерно повысит нагрузку:

THREAD\_PER\_PROCESSOR=10

"false" - если OpenId провайдер не поддерживает PKCE:

USE\_PKCE=true

Необходимый параметр, если внешний сервис Jira заблокирован для исходящих подключений. Указанное значение в секундах, время через которое Test IT инициирует входящее подключение к Jira для синхронизации данных:

SYNC\_RESULT\_LINKS\_EVERY\_SEC=120

Период хранения бизнес логов по действиям пользователей в Elasticsearch, максимальный объем хранения логов (GB), имя индексов логов в elasticsearch:

EVENT\_LOG\_MAX\_AGE=30d

EVENT\_LOG\_MAX\_SIZE=50gb

ELASTICSEARCH\_LOGS\_INDEX=action\_logs

Если вы меняли порты по умолчанию для контейнера webapi то нужно изменить следующий параметр:

WEBAPI\_URL=http://webapi

### Настройка внешних подключений

#### Настройка внешнего подключения RabbitMQ

Версия внешнего сервиса должна совпадать с версией, указанной в файле docker-compose.yml.

1. Укажите в файле .env для следующих параметров значения, установленные вами при настройке RabbitMQ (ниже указаны значения по умолчанию):

RABBITMQ\_DEFAULT\_USER=testit

RABBITMQ\_DEFAULT\_PASS=password

RABBITMQ\_DEFAULT\_VHOST=testitrabbit

RABBITMQ\_DEFAULT\_HOST=external-server (где external-server - ip-адрес или DNS-имя вашего сервера с RabbitMQ)

RABBITMQ\_DEFAULT\_PORT=5672

RABBITMQ\_AUTH\_MODE=plain

RABBITMQ\_CLIENT\_CERT\_PATH=/etc/rabbitmq/ssl/client/testit.pfx

\#RABBITMQ\_CLIENT\_CERT\_PASSPHRASE=

1. В файле docker-compose.yml закомментируйте секцию с сервисом rabbitmq, зависимости от него других контейнеров (все упоминания rabbitmq в блоках depends\_on) и rabbit-volume, rabbitmq-configuration-volume, rabbitmq-certificates-volume в списке volumes.
2. Перезапустите систему Test IT:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans

#### Настройка внешнего подключения стека Logstash и Kibana (ELK)

Версия внешнего сервиса должна совпадать с версией, указанной в файле docker-compose.yml.

1. При настройке стека ELK укажите в .env файле следующие параметры соответственно вашей конфигурации:

ELASTICSEARCH\_CONNECTION\_STRING=http://external-server:9200 (где external-server - ip-адрес или DNS-имя вашего сервера с Elasticsearch)

ELASTICSEARCH\_INDEX= (заданное вами имя индекса для TestIT)

ELASTICSEARCH\_LOGS\_INDEX= (заданное вами имя индекса логов)

1. В файле docker-compose.yml закомментируйте секции с сервисами Elasticsearch, Logstash, Kibana, зависимости от него других контейнеров (все упоминания elasticsearch, logstash, kibana в блоках depends\_on) и elastic-volume в списке volumes.
2. Перезапустите систему Test IT:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans

#### Настройка внешнего подключения minio и avatars.minio

Версия внешнего сервиса должна совпадать с версией, указанной в файле docker-compose.yml.

Вы можете использовать одну базу данных для сервисов minio и avatars.minio.

1. Создайте два бакета. Например, bucket1 на замену сервису minio и bucket2 на замену сервису avatars.minio.
2. Для каждого из бакетов создайте пару Access Key и Secret Key.
3. В .env файле:
   1. Для сервиса minio установите следующие значения переменных:

AWS\_ACCESS\_KEY: (Access key, установленный для bucket1)

AWS\_SECRET\_KEY: (Secret Key, установленный для bucket1)

AWS\_CONNECTION\_STRING: http://external-server:9000 (где external-server - ip-адрес или DNS-имя вашего сервера с minio)

FILE\_BUCKET\_NAME: (в данном примере, bucket1)

*
  1. Для сервиса avatars.minio установите следующие значения переменных:

AVATARS\_AWS\_ACCESS\_KEY: (Access key, установленный для bucket2)

AVATARS\_AWS\_SECRET\_KEY: (Secret Key, установленный для bucket2)

AVATARS\_AWS\_CONNECTION\_STRING: http://external-server:9000

*
  1. Добавьте для сервиса avatars.minio следующую переменную:

AVATARS\_FILE\_BUCKET\_NAME: (в данном примере, bucket2)

1. Добавьте в файл docker-compose.yml в секции avatars.api в разделе environment следующую строку:

AWSS3Server\_\_FileBucketName: “${AVATARS\_FILE\_BUCKET\_NAME}”

1. В файле docker-compose.yml закомментируйте секцию с сервисами minio и avatars.minio, зависимости от них других контейнеров (все упоминания сервисов в блоках depends\_on), и их вольюмы (minio-export-volume и minio-data-volume) в списке volumes.
2. Перезапустите систему Test IT:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans

#### Настройка внешнего подключения базы данных Redis

Версия внешнего сервиса должна совпадать с версией, указанной в файле docker-compose.yml.

1. При настройке внешней базы данных Redis установите следующий параметр:

appendonly yes

1. В файле docker-compose.yml закомментируйте или удалите секцию с сервисом БД (auth\_cache), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках depends\_on), и его вольюм (auth-cache-volume) в списке volumes в файле docker-compose.yml.
2. В .env файле укажите данные для подключения к внешней БД, где external-server - IP или DNS-имя хоста, на котором установлен Redis (без указания протокола и порта). AUTH\_CACHE\_CONNECTION\_STRING=external-server
3. Перезапустите систему Test IT:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans

#### Настройка внешнего подключения базы данных InfluxDB

Версия внешнего сервиса должна совпадать с версией, указанной в файле docker-compose.yml.

1. При настройке внешней базы данных InfluxDB установите следующие параметры:

* max-series-per-database = 0
* max-values-per-tag = 0

1. Закомментируйте или удалите секцию с сервисом БД (influxdb), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках depends\_on), и его вольюм (influx-volume) в списке volumes в файле docker-compose.yml.
2. В .env файле укажите данные для подключения к внешней БД, где external-server - IP-адрес или DNS-имя вашего сервера с InfluxDB.

INFLUX\_CONNECTION\_STRING=http://external-server:8086

1. Перезапустите систему Test IT:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120 --remove-orphans

#### Настройка внешнего подключения базы данных PostgreSQL

Версия внешнего сервиса должна совпадать с версией, указанной в файле docker-compose.yml.

Подготовьте внешнюю БД (PostgreSQL) для каждого сервиса (имена баз данных: testitdb, authdb, avatarsdb):

yum install postgresql-contrib

psql -U postgres

create database testitdb;

create user tester with encrypted password 'tester';

grant all privileges on database testitdb to tester;

\connect testitdb;

CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;

Для остальных БД (authdb, avatarsdb) нужно выполнить скрипт ниже подставив название БД вместо testitdb:

create database authdb;

grant all privileges on database authdb to tester;

\connect authdb;

CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;

create database avatarsdb;

grant all privileges on database avatarsdb to tester;

\connect avatarsdb;

CREATE EXTENSION if not exists "uuid-ossp" SCHEMA public;

Закомментируйте или удалите секцию с сервисом БД (authdb, avatars.db, db), который будет заменен на внешний сервис, зависимости от него других контейнеров (все упоминания сервиса БД в блоках depends\_on), и его вольюмы (authdb-volume, db-volume, avatars.db-volume) в списке volumes в файле docker-compose.yml.

В .env файле укажите данные для подключения к внешней БД. (ip-external server/dns-name, port, login, password). В Host можно указать отдельные СУБД или одну и ту же СУБД для каждой БД.

DB\_CONNECTION\_STRING=Host=external\_server1;Port=5432;Database=testitdb;Username=tester;Password=tester;Pooling=true;Maximum Pool Size=130

\#POSTGRES\_DB=testitdb

\#POSTGRES\_USER=postgres

\#POSTGRES\_PASSWORD=F1rstL0g0N!

...

AUTH\_CONNECTION\_STRING=Host=external\_server2;Port=5432;Database=authdb;Username=tester;Password=tester;Pooling=true;Maximum Pool Size=130

\#POSTGRES\_AUTH\_DB=authdb

\#POSTGRES\_AUTH\_USER=postgres

\#POSTGRES\_AUTH\_PASSWORD=F1rstL0g0N!

...

AVATARS\_CONNECTION\_STRING=Host=external\_server3;Port=5432;Database=avatarsdb;Username=tester;Password=tester

\#POSTGRES\_AVATARS\_DB=avatarsdb

\#POSTGRES\_AVATARS\_USER=postgres

\#POSTGRES\_AVATARS\_PASSWORD=F1rstL0g0N!

Выполните установку Test IT:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120

## Обновление ПО

Если в .env и .yml файлах используются ваши пользовательские значения, перенесите их в файлы .env и .yml новой версии Test IT. Все значения в файлах .env и .yml новых версий Test IT заменяются на дефолтные при обновлении.

Если вы обновляетесь с версии до 1.0.7 (например 1.0.6), сделайте бэкап, обновитесь на версию 1.0.7

Если вы обновляетесь с версии до 2.4, сделайте бэкап, обновитесь до версии 2.4, после чего обновитесь на версию 3.5

### **Подготовка**

Перед обновлением рекомендуем создать резервную копию установленной системы (файлы "docker-compose.yml" и "backup.sh" должны находиться в одной директории):

\# В папке с версией 3.4

chmod +x backup.sh

./backup.sh docker-compose.yml prod

### **Online обновление**

Чтобы перенести информацию из volumes со старой версии на новую, имена проектов обеих версий должны совпадать. В наших примерах проект называется "prod".

Создайте новую директорию, скачайте и распакуйте в ней файл для онлайн установки.

​В командной строке перейдите в директорию с версией 3.5 и выполните:

\# В папке с версией 3.5

docker-compose -f docker-compose.yml --project-name prod up -d --remove-orphans

### **Автономное обновление**

Чтобы перенести информацию из volumes со старой версии на новую, имена проектов обеих версий должны совпадать. В наших примерах проект называется "prod".

Создайте новую директорию, скачайте и распакуйте в ней файл для автономной установки.

В командной строке перейдите в директорию 3.5, распакуйте архив для автономной установки и выполните:

\# В папке с версией 3.5

docker load -i images.tar.gz

docker-compose -f docker-compose.yml --project-name prod up -d --remove-orphans

## Резервное копирование

### **Создание резервных копий**

Продукт будет остановлен на время создания резервной копии. Не следует создавать резервные копии из под sudo.

Перед выполнением скрипта на создание резервной копии следует перейти в директорию, которая содержит docker-compose.yml файл с настройками текущей версии системы.

Для создания резервной копии необходимо выполнить:

chmod +x scripts/backup.sh

scripts/backup.sh docker-compose.yml prod

Система будет запущена после окончания процесса. В рабочей директории будет создан архив с резервной копией. Формат имени файла архива: backup\_{день}\_{месяц}\_{год}.tar. Например, backup\_21\_05\_2019.tar.

### **Восстановление из резервной копии**

Продукт будет остановлен на время восстановления из резервной копии.

Перед выполнением скрипта на восстановление из резервной копии следует перейти в директорию, которая содержит docker-compose.yml и .env файлы с настройками текущей версии системы.

Для восстановления из резервной копии необходимо выполнить:

chmod +x scripts/restore.sh

scripts/restore.sh docker-compose.yml prod backup\_21\_05\_2019.tar

Система будет запущена после окончания процесса.

При переносе продукта на другой сервер следует предварительно установить Test IT на новом сервере с настройками по умолчанию и затем восстановить данные системы из резервной копии. Для наилучшей совместимости на новом сервере рекомендуется устанавливать Test IT той же версии, которая содержится в резервной копии, переносимой из исходного сервера.

​

## Логирование пользовательских действий

Необходимо выставить параметр vm.max\_map\_count=262144 и vm.overcommit\_memory=1:

echo 'vm.max\_map\_count=262144' >> /etc/sysctl.conf

echo 'vm.overcommit\_memory = 1' >> /etc/sysctl.conf

sysctl -p

Для включения опции логирования пользовательских действий, замените docker-compose.yml на содержимое docker-compose.elk.yml:

cd \~/testit

cp docker-compose.yml docker-compose.yml.bak # резервная копия

cp docker-compose.elk.yml docker-compose.yml

и выполните шаги из Инструкция по установке в Docker Compose или Руководства по обновлению.

## Настройка HTTPS

Перед настройкой https необходимо провести базовую настройку и установку Test IT.

Откройте 443 порт, подробности смотрите в инструкции по установке.

В .env файле раскомментируйте переменные SSL\_CERTIFICATE и SSL\_CERTIFICATE\_KEY.

\## internal certificate path

SSL\_CERTIFICATE=/etc/nginx/ssl/testit.crt

SSL\_CERTIFICATE\_KEY=/etc/nginx/ssl/testit.key

\#REDIRECT\_TO\_HTTPS=true

Раскомментируйте REDIRECT\_TO\_HTTPS для редиректа http на https.

Чтобы сервис frontend был доступен по https протоколу, пропишите 443 порт в docker-compose.yml файле:

services:

frontend:

image: "${DOCKER\_REGISTRY}/frontend:${CONTAINER\_VERSION}"

ports:

\- 80:80

\- 443:443

...

Подготовьте файлы с сертификатом и ключом. Для этого дайте им имена testit.crt и testit.key.

Имена файлов сертификатов должны соответствовать значению переменных SSL\_CERTIFICATE и SSL\_CERTIFICATE\_KEY в .env файле.

Cкопируйте подготовленные файлы в хранилище сертификатов:

certs=$(docker inspect prod\_ssl-volume --format '\{{ .Mountpoint \}}')

cp testit.crt ${certs}/

cp testit.key ${certs}/

Примените изменения, выполнив команду:

docker-compose -f docker-compose.yml --project-name prod up --detach --timeout 120
