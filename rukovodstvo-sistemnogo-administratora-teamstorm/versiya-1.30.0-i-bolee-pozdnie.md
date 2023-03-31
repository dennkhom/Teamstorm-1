# Версия 1.30.0 и более поздние

## Назначение документа

Документ описывает действия системного администратора по установке и настройке ПО TeamStorm версии 1.30.0 и выше.

## **Установка программного ПО**

Установка ПО TeamStorm осуществляется только после предварительной установки [ПО Test IT](https://testit.software/versions/).

Установка Test IT описана в [документации Test IT](https://docs.testit.software/installation-guide/).

ПО TeamStorm необходимо устанавливать на тот же хост, на который установлено ПО Test IT.

### Установка, перезапуск и удаление в Docker Compose

#### **Требования**

​[Docker Engine 20.10.17 и выше](https://docs.docker.com/engine).

[Docker Compose 2.10.0 и выше.](https://docs.docker.com/compose)

[Test IT 4.0.1 и выше](https://testit.software/versions/).

{% hint style="warning" %}
Test IT 4.0.1 и 4.0.2 для версии TeamStorm 1.30.0.

Test IT 4.1 и выше для версии TeamStorm 1.31.
{% endhint %}

#### **Состав поставки**

`images.tar` - архив с образами (только в архиве для автономной установки)**.**

Состав поставки TeamStorm:

* `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам TeamStorm;
* `docker-compose.cwm.yml` - конфигурационный файл Docker Compose.

#### **Подготовка Test IT для установки TeamStorm**

1. Если у вас еще не установлено ПО Test IT, загрузите и установите [Test IT](https://testit.software/versions/) в соответствии с документацией Test IT.
2. Если у вас установлено ПО Test IT с версией ниже рекомендуемой, выполните обновление согласно [требованиям](versiya-1.30.0-i-bolee-pozdnie.md#trebovaniya).
3.  Настройте поддержку TeamStorm в Test IT, заменив значение переменной `CWM_ENABLED`:

    ```
    # Отредактируйте файл переменных окружения Testit:
    vi ./testit/.env
    <<<
    CWM_ENABLED="false"
    >>>
    CWM_ENABLED="true"

    ```

#### **Подготовка**

1. Измените значения переменных по умолчанию в .env-файле.
2.  Задайте параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:

    ```bash
    echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
    sysctl -p
    ```
3. Заблокируйте все порты, кроме порта 80, необходимого для доступа к пользовательскому интерфейсу.
4.  **Опционально:** для обслуживания системы посредством протокола SSH, необходимо открыть порт 22 (может быть переназначено на конкретной конфигурации). Для работы по HTTPS необходимо открыть порт 443. Пример открытия доступа к портам для CentOS 8:

    ```bash
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    firewall-cmd --zone=public --add-port=22/tcp --permanent
    firewall-cmd --zone=public --add-port=443/tcp --permanent
    firewall-cmd --reload
    ```

#### **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v1.30.0`.
2.  Выполните следующие команды:

    ```bash
    tar -xzvf ts_distro_v1.30.0.tgz
    chmod +x setup.sh
    ./setup.sh
    ```

#### **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

```bash
cd teamstorm_v1.30.0
docker-compose -f docker-compose.cwm.yml --project-name teamstorm restart --timeout 120
```

#### Удаление системы

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```bash
cd teamstorm_v1.30.0
docker-compose -f docker-compose.cwm.yml --project-name teamstorm down --volumes --timeout 120
```

Чтобы сохранить информацию для последующего использования, выполните команду без флага `--volumes`.

## Описание .env файла

Репозиторий для скачивания образов установки Teamstorm:

```shell
CWM_DOCKER_REGISTRY="docker.testit.ru/teamstorm"
```

Текущая версия программы:

```shell
CWM_CONTAINER_VERSION="1.30.0"
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

Параметры подключения к БД, при установке внешней БД, поменять на свои значения:

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

Системные параметры, оставить без изменений:

```shell
ASPNETCORE_ENVIRONMENT="Production"
FILE_BUCKET_NAME="teamstorm"
```
