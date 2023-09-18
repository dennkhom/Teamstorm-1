# Чистая установка и обновление

## Назначение документа

Документ описывает действия системного администратора по чистой установке или обновлению TeamStorm v. 2.33.0 и выше.

## Сведения об установке

Чистая установка и обновление осуществляются совместно с установкой [ПО Test IT](https://testit.software/versions/).

Данный вариант установки не подойдет, если на хосте уже установлен Test IT.

{% hint style="danger" %}
Если у вас уже установлено ПО Test IT, то ПО TeamStorm по процедуре чистой установки рекомендуется устанавливать на другой хост во избежание потери информации в Test IT
{% endhint %}



Установка Teamstorm на Test IT описана

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

Установка TeamStorm  на Test IT описана&#x20;

## **Автономная установка**

Данный тип установки поможет установить или обновить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v2.33.0`.
2.  Перейдите в папку `teamstorm`:

    &#x20;`$ cd teamstorm`
3. Запустите скрипт установки: \
   `$ sh ./setup.sh`
4. Дождитесь полной установки ПО.
5. Проверьте корректность установки.&#x20;

## Проверка корректности установки

Для проверки корректности установки:

1. Убедитесь в том, что в Системе предсоздан пользователь с ролью администратора. Авторизуйтесь под учетной записью администратора (cwm\_admin).
2. Убедитесь в том, что [лицензии TeamStorm добавлены](../../../rukovodstvo-administratora-teamstorm-po-dobavleniyu-polzovatelei.md#prosmotr-informacii-o-licenziyakh).&#x20;
3. Убедитесь в том, что в системе есть другие пользователи, или [добавьте нового пользователя](../../../rukovodstvo-administratora-teamstorm-po-dobavleniyu-polzovatelei.md#dobavlenie-polzovatelei) (например, с именем user1).
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
