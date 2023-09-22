# Установка на Test IT

## Назначение документа

Документ описывает действия системного администратора по установке и настройке ПО TeamStorm v. 2.33.0 и выше в случае если у вас установлено только ПО Test IT и необходимо установить ПО TeamStorm на тот же хост.

## **Сведения об установке**

Установка ПО TeamStorm осуществляется только после предварительного обновления [ПО Test IT](https://testit.software/versions/).

{% hint style="info" %}
Для обновления TeamStorm до версии 2.33.0 требуется обновление Test IT до версии 4.4.0 или выше, рекомендуется последняя версия

Для обновления TeamStorm до версии 2.33.0 необходимо предварительно обновить TeamStorm до версии 2.0.0
{% endhint %}

Обновление Test IT описано в [документации Test IT](https://docs.testit.software/installation-guide/).

ПО TeamStorm необходимо устанавливать на тот же хост, на который установлено ПО Test IT.

## Установка, перезапуск и удаление в Docker Compose

### **Требования**

​[Docker Engine 20.10.17 и выше](https://docs.docker.com/engine).

[Docker Compose 2.10.0 и выше.](https://docs.docker.com/compose)

[Test IT 4.4.0 и выше, рекомендуется последняя версия](https://testit.software/versions/).

### **Состав поставки**

* `images.tar` - архив с образами (только в архиве для автономной установки)**.**
* `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам TeamStorm;
* `docker-compose.yml` - конфигурационный файл Docker Compose;
* setup.sh - скрипт для упрощенного развертывания TeamStorm и Test IT;
* setup\_teamstorm.sh - скрипт для автоматического развертывания TeamStorm

### **Установка и настройка Test IT**

1. &#x20;**Опционально**: обновите ПО Test IT в соответствии с документацией Test IT.
2.  Настройте поддержку TeamStorm в Test IT, заменив значение переменной `CWM_ENABLED`:

    ```shell
    # Отредактируйте файл переменных окружения Test IT:
    vi ./testit/.env
    <<<
    CWM_ENABLED="false"
    >>>
    CWM_ENABLED="true"
    ```
3.  Следующие переменные в конфигурационных файлах `testit` и `teamstorm` должны совпадать:

    | `testit/.env`                 |        `teamstorm/.env`       | Комментарий                                                                  |
    | ----------------------------- | :---------------------------: | ---------------------------------------------------------------------------- |
    | FRONTEND\_URL                 |       CWM\_FRONTEND\_URL      | Например, "[https://teamstorm.mycompany.ru](https://teamstorm.mycompany.ru)" |
    | CWM\_S3\_BUCKET\_SECRET\_KEY  |  CWM\_S3\_BUCKET\_SECRET\_KEY | Переменная не должна содержать символ `$`                                    |
    | WIKI\_S3\_BUCKET\_SECRET\_KEY | WIKI\_S3\_BUCKET\_SECRET\_KEY | Переменная не должна содержать символ `$`                                    |
4.  Убедитесь, что сервис `auth`  имеет настройку для редактирования ролей:

    ```shell
    $ vi ./testit/docker-compose.yml
    ...
    auth:
    ...
        environment:
    >>>   CAN_EDIT_SYSTEM_ROLES: "${CAN_EDIT_SYSTEM_ROLES:-true}"
    ```

При обновлении с Test IT 4.3.0 на Test IT 4.4.0 дополнительные действия не требуются.

### **Подготовка**

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

### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v2.33.0`.
2. Создайте сеть и кластер вручную. При этом обязательно использовать ключ `--remove-orphans`:

```bash
cd ${PROJECT_HOME}/teamstorm
docker network create yoonion_network
docker compose -p teamstorm -f docker-compose.yml up -d --remove-orphans
cd ${PROJECT_HOME}/testit
docker compose -p testit -f docker-compose.yml up -d
```

или воспользуйтесь скриптом автоматического развертывания из поставки:

```
tar -xzvf teamstorm_v3.0.0.tgz
cd ${PROJECT_HOME}/teamstorm
chmod +x setup.sh
./setup_teamstorm.sh
```

### **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

```bash
cd ${PROJECT_HOME}/teamstorm
docker-compose -f docker-compose.yml -p teamstorm restart
```

### Удаление системы

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```bash
cd teamstorm_v2.33.0
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
CWM_CONTAINER_VERSION="2.33.0"
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

Системные параметры оставить без изменений:

```shell
ASPNETCORE_ENVIRONMENT="Production"
FILE_BUCKET_NAME="teamstorm"
```

## Настройка параметров почтового сервера для уведомлений

Используйте доверенные сертификаты для соединения с использованием самоподписных сертификатов. Для этого необходимо положить файл, содержащий сгенерированный сертификат в формате PEM, в `docker volume trusted-certificates-volume`.&#x20;

Например:

```
$ ls /var/lib/docker/volumes/teamstorm_trusted-certificates-volume/_data/
/var/lib/docker/volumes/teamstorm_trusted-certificates-volume/_data/host.pem
MAIL_SERVER_USE_CUSTOM_TRUSTED_CA_BUNDLE="true"
```

Внесите изменения в env-файл:

1. Укажите хост размещения TeamStorm: `CWM_FRONTEND_URL="https://teamstorm.contoso.com"`
2. Для использования полной проверки без исключений используйте значение `MAIL_SERVER_USE_CUSTOM_TRUSTED_CA_BUNDLE="false"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USE_CUSTOM_TRUSTED_CA_BUNDLE="false"`

3. При пустом MAIL\_SERVER\_HOST значении сервер не отправляет сообщения. Укажите хост почтового сервера. Например: `MAIL_SERVER_HOST="smtp.mail.io"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_HOST=""`

4. Укажите порт почтового сервера. Например: `MAIL_SERVER_PORT="25"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_PORT="1025"`

5. Укажите сервисный аккаунт обратный адрес которого будет указан в заголовке сообщения. Например: `MAIL_SERVER_FROM="deamon@mail.io"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_FROM="deamon@service.com"`

6. Укажите имя сервисного аккаунта для отображения . Например: `MAIL_SERVER_DISPLAY_NAME="Teamstorm Mail Notification"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_DISPLAY_NAME="Mail Deamon"`

7. Укажите имя пользователя для аутентификации на сервере SMTP/IMAP . Например: `MAIL_SERVER_USER_NAME="username"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USER_NAME=" "`

8. Укажите пароль для аутентификации. Например:  `MAIL_SERVER_PASSWORD="password"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_PASSWORD=""`

9. Для подключения к почтовому серверу через STARTTLS укажите: `MAIL_SERVER_USE_START_TLS="true"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USE_START_TLS="false"`

10. &#x20;Для подключения к почтовому серверу через SSL укажите: `MAIL_SERVER_USE_SSL="true"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USE_SSL="false"`

11. Укажите идентификатор часового пояса, например: `MAIL_SERVER_TZ="Europe/Moscow"`

&#x20;       По умолчанию установлено московское время: `MAIL_SERVER_TZ="Europe/Moscow"`

&#x20;       Таблица идентификаторов временных зон:  [https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List)

12. После внесения изменений перезапустите систему.&#x20;

## Проверка корректности установки

Для проверки корректности установки:

1. Убедитесь в том, что в Системе предсоздан пользователь с ролью администратора. Авторизуйтесь под учетной записью администратора (cwm\_admin).
2. Убедитесь в том, что [лицензии TeamStorm добавлены](../../../../rukovodstvo-administratora-teamstorm-po-dobavleniyu-polzovatelei.md#prosmotr-informacii-o-licenziyakh).&#x20;
3. Убедитесь в том, что в системе есть другие пользователи, или [добавьте нового пользователя](../../../../rukovodstvo-administratora-teamstorm-po-dobavleniyu-polzovatelei.md#dobavlenie-polzovatelei) (например, с именем user1).
4. [Создайте пространство](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-prostranstvami/sozdanie-prostranstva).
5. [Создайте папку](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-papkami/sozdanie-papok).
6. [Создайте задачу](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-zadachami/sozdanie-zadachi).
7. [Создайте страницу](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-razdelom-stranicy/sozdanie-stranicy).
8. Перейдите в настройки пространства, [добавьте в пространство пользователя](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm#dobavlenie-i-udalenie-polzovatelei-i-grupp-polzovatelei-v-prostranstve), созданного на шаге 3.
9. Перейдите в созданную задачу и [отредактируйте](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-zadachami/redaktirovanie-zadachi) ее:
   * поменяйте ее статус;
   * выберите ответственным пользователя, созданного на шаге 3.
   * добавьте описание;
   * добавьте вложение.
10. Убедитесь в том, что пользователю, созданному на шаге 3, пришло почтовое уведомление.&#x20;

Установка выполнена корректно, если все шаги проверки выполняются.&#x20;