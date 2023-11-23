# Чистая установка и обновление

## Назначение документа

Документ описывает действия системного администратора по чистой установке или обновлению TeamStorm v. 2.33.0 и выше.

## Сведения об установке

Чистая установка и обновление осуществляются совместно с автоматической установкой [ПО Test IT](https://testit.software/versions/).

{% hint style="danger" %}
Данный вариант установки не подойдет, если на хосте уже установлен Test IT. Возможна потеря данных
{% endhint %}

{% hint style="info" %}
Обновление TeamStorm до версии 2.33.0 возможно с версии 2.0.0&#x20;

Если ваша версия ниже, сперва обновитесь до версии 2.0.0
{% endhint %}

Если у вас уже установлено ПО Test IT, следуйте инструкциям из раздела [Установка на Test IT](https://docs.teamstorm.io/rukovodstva/rukovodstvo-sistemnogo-administratora-teamstorm/versiya-2.33.0/ustanovka-na-test-it).

## **Требования**

​[Docker Engine 20.10.17 и выше](https://docs.docker.com/engine).

[Docker Compose 2.10.0 и выше.](https://docs.docker.com/compose)

## **Состав поставки**

Архив автономной установки содержит папки:

1. `teamstorm`, которая содержит:
   * `images.tar` - архив с образами (только в архиве для автономной установки)**.**
   * `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам TeamStorm;
   * `docker-compose.yml` - конфигурационный файл Docker Compose;
   * `setup.sh` - скрипт для упрощенного развертывания TeamStorm и Test IT;
   * `setup_teamstorm.sh` - скрипт для автоматического развертывания TeamStorm
2. `testit` с соответствующим набором компонентов, необходимых для установки ПО Test IT.

Установка TeamStorm при уже установленном Test IT описана в разделе [Установка на Test IT](https://docs.teamstorm.io/rukovodstva/rukovodstvo-sistemnogo-administratora-teamstorm/versiya-2.33.0/ustanovka-na-test-it).

## Подготовка к установке

Для обновления TeamStorm до версии 2.33.0 необходимо предварительно обновить TeamStorm до версии 2.0.0.

Перед обновлением рекомендуется создать резервную копию TeamStorm и Test IT и проверить соответствие наименований проектов в `docker-compose`. Возможна потеря данных при неправильном обновлении.

Для проверки выполнить:

```
$ docker compose ls 
NAME               STATUS             CONFIG FILES 
teamstorm          running(20)        ./deploy/offline_build/teamstorm/docker-compose.yml 
testit             running(20)        ./deploy/offline_build/testit/docker-compose.yml
```

## **Автономная установка**

Данный тип установки поможет установить или обновить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v2.33.0`.
2.  Перейдите в папку `teamstorm`:

    &#x20;`$ cd teamstorm`
3. Запустите скрипт установки: \
   `$ sh ./setup.sh`
4. Дождитесь полной установки ПО.
5. Проверьте [корректность установки](https://docs.teamstorm.io/rukovodstva/rukovodstvo-sistemnogo-administratora-teamstorm/versiya-2.33.0/ustanovka-na-test-it#proverka-korrektnosti-ustanovki).&#x20;

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

## Проверка корректности установки

Для проверки корректности установки:

1. Убедитесь в том, что в Системе предсоздан пользователь с ролью администратора. Авторизуйтесь под учетной записью администратора (cwm\_admin).
2. Убедитесь в том, что [лицензии TeamStorm добавлены](../../../rukovodstvo-administratora-teamstorm-po-dobavleniyu-polzovatelei/#prosmotr-informacii-o-licenziyakh).&#x20;
3. Убедитесь в том, что в системе есть другие пользователи, или [добавьте нового пользователя](../../../rukovodstvo-administratora-teamstorm-po-dobavleniyu-polzovatelei/#dobavlenie-polzovatelei) (например, с именем user1).
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

Установка выполнена корректно, если все шаги проверки выполняются.

## **Перезапуск системы**

Для перезапуска системы воспользуйтесь следующей командой:

```bash
cd ${PROJECT_HOME}/teamstorm
docker-compose -f docker-compose.yml -p teamstorm restart
```

## Удаление системы

Для полного удаления системы и ее данных необходимо выполнить следующую команду:

```bash
cd teamstorm_v2.33.0
docker-compose -f docker-compose.yml --project-name teamstorm down --volumes --timeout 120
```

Чтобы сохранить информацию для последующего использования, выполните команду без флага `--volumes`.
