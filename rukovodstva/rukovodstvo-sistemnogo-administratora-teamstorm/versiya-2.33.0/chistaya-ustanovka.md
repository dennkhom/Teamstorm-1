# Чистая установка

## Назначение документа

Документ описывает действия системного администратора по чистой установкеTeamStorm v. 2.33.0 и выше.

## **Установка программного ПО**

Чистая установка  осуществляется соместно с установкой [ПО Test IT](https://testit.software/versions/).

Настройка Test IT описана в [документации Test IT](https://docs.testit.software/installation-guide/).

{% hint style="danger" %}
Если у вас уже установлено ПО Test IT, то ПО TeamStorm по процедуре чистой установки необходимо устанавливать на другой хост во избежание конфликтов
{% endhint %}

## **Требования**

​[Docker Engine 20.10.17 и выше](https://docs.docker.com/engine).

[Docker Compose 2.10.0 и выше.](https://docs.docker.com/compose)

## **Состав поставки**

Архив автономной установки содержит папки:

1. `teamstorm`, которая содержит:
   * `images.tar` - архив с образами (только в архиве для автономной установки)**.**
   * `.env` - конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам TeamStorm;
   * `docker-compose.yml` - конфигурационный файл Docker Compose;
   * setup.sh - скрипт для упрощенного развертывания TeamStorm и Test IT;
   * setup\_teamstorm.sh - скрипт для автоматического развертывания TeamStorm
2. `testit` с соответствующим набором компонентов, необходимых для установки ПО Test IT.

## **Автономная установка**

Данный тип установки поможет установить продукт, если сервер изолирован от сети Internet и нет возможности получить Docker-образы с публичных репозиториев.

1. Распакуйте содержимое архива автономной установки, например, в папку `~/teamstorm_v2.33.0`.
2.  Перейдите в папку `teamstorm`:

    &#x20;`$ cd teamstorm`
3. Запустите скрипт установки: \
   `$ sh ./setup.sh`
4. Дождитесь полной установки ПО.
5. Проверьте корректность установки.&#x20;

## Проверка корректности установки

Для проверки корректности установки:

1. Убедитесь в том, что в Системе предсоздан пользователь с ролью администратора. Авторизуйтесь под учетной записью администратора.
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

Установка выполнена корректно, если все шаги проверки выполняются
