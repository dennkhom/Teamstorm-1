# Обновление Teamstorm

{% hint style="warning" %}
Перед обновлением рекомендуем делать резервное копирование во избежание потери данных.
{% endhint %}

1.  Если у вас уже был ранее установлен TeamStorm, то перенесите директории с артефактами `testit` и `teamstorm` в другое месторасположение, например:

    ```shell
    user@server: ~ $ mkdir /tmp/teamstorm_previous
    user@server: ~ $ mv testit/ teamstorm/ /tmp/teamstorm_previous
    ...
    ```
2.  Распакуйте архив сборки, например:

    ```shell
    user@server: ~ $ tar -xzvf teamstorm_full_v2.33.3.tgz .
    ...
    ```
3.  После завершения разархивирования сравните файлы конфигураций `.env` и `docker-compose.yml` с файлами из предыдущей версии и внесите необходимые изменения, например:

    ```shell
    user@server: ~ $ diff testit/.env /tmp/teamstorm_previous/testit/.env
    << FRONTEND_URL="http://localhost"
    >> FRONTEND_URL="https://teamstorm.mycompany.io"
    vi testit/.env
    ...
    FRONTEND_URL="teamstorm.mycompany.io"
    user@server: ~ $ diff testit/docker-compose.yml /tmp/teamstorm_previous/testit/docker-compose.yml
    user@server: ~ $ diff teamstorm/.env /tmp/teamstorm_previous/teamstorm/.env
    user@server: ~ $ diff teamstorm/docker-compose.yml /tmp/teamstorm_previous/teamstorm/docker-compose.yml
    ```
4.  Зайдите в любую из директорий `testit` или `teamstorm` и запустите скрипт установки:

    ```shell
    user@server: ~ $ cd teamstorm
    user@server: ~/teamstorm/ $ sh ./setup.sh
    ```

Установка обычно занимает не более 5 минут.

Пожалуйста, дождитесь завершения и перейдите к проверке работоспособности приложения.
