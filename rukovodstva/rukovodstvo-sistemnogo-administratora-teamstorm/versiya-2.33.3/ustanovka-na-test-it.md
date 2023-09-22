# Установка на Test IT

Используйте эту инструкцию, если у вас ранее уже было установлено программное обеспечение Test IT и вы хотите доустановить TeamStorm.

{% hint style="warning" %}
Перед обновлением рекомендуем делать резервное копирование во избежание потери данных.
{% endhint %}

1.  Переместите артефакты предыдущей установки Test IT во временную директорию, например:

    ```shell
    user@server: ~ $ mkdir /tmp/testit_previous
    user@server: ~ $ mv ~/testit /tmp/testit_previous
    ```
2.  Положите и распакуйте архив в целевую директорию, например:

    ```shell
    user@server: ~ $ scp somewhere:~/teamstorm_full_v2.33.3.tgz .
    user@server: ~ $ tar -xzvf teamstorm_full_v2.33.3.tgz
    ./teamstorm/
    ./teamstorm/scripts/
    ./teamstorm/scripts/postgres-init.sql
    ./teamstorm/scripts/db_backup.sh
    ./teamstorm/scripts/db_restore.sh
    ./teamstorm/configs/
    ./teamstorm/configs/rabbitmq_enabled_plugins
    ./teamstorm/configs/postgres_exporter.yml
    ./teamstorm/docker-compose.yml
    ./teamstorm/.env
    ./teamstorm/setup_teamstorm.sh
    ./teamstorm/setup.sh
    ./teamstorm/images.list
    ./teamstorm/images.tar
    ...
    ```
3.  Сравните файлы установок предыдущей установки Test IT с новой версией и синхронизируйте ранее выполненные изменения. Новые переменные можно пока оставить без изменений. Переменную `CWM_ENABLED` оставить в значении  `true`, например:

    ```shell
    user@server: ~ $ diff testit/docker-compose.yml /tmp/testit_previous/testit/docker-compose.yml

    <<      - 443:8443/tcp
    >>      # - 443:8443/tcp

    >> FRONTEND_URL="https://teamstorm.mycompany.io"

    << CWM_ENABLED="true"

    vi testit/.env
    ...
    FRONTEND_URL="teamstorm.mycompany.io"
    CWM_ENABLED="true"

    user@server: ~ $ diff testit/.env /tmp/testit_previous/testit/.env
    ...
    << FRONTEND_URL="http://localhost"
    >> FRONTEND_URL="https://teamstorm.mycompany.io"

    << CWM_ENABLED="true"

    vi testit/.env
    ...
    FRONTEND_URL="teamstorm.mycompany.io"
    CWM_ENABLED="true"
    ```
4.  Теперь нужно убедиться в соответствии следующих переменных

    | `testit/.env`                 |        `teamstorm/.env`       | Комментарий                                                                  |
    | ----------------------------- | :---------------------------: | ---------------------------------------------------------------------------- |
    | FRONTEND\_URL                 |       CWM\_FRONTEND\_URL      | Например: "[https://teamstorm.mycompany.ru](https://teamstorm.mycompany.ru)" |
    | CWM\_S3\_BUCKET\_SECRET\_KEY  |  CWM\_S3\_BUCKET\_SECRET\_KEY | Переменная не должна содержать символ `$`                                    |
    | WIKI\_S3\_BUCKET\_SECRET\_KEY | WIKI\_S3\_BUCKET\_SECRET\_KEY | Переменная не должна содержать символ `$`                                    |
5.  Если вы уверены, что выполнили все предыдущие шаги корректно, то перейдите в директорию `teamstorm` или `testit` и запустите скрипт установки, например:

    ```shell
    user@server: ~ $ cd teamstorm
    user@server: ~/teamstorm/ $ sh ./setup.sh
    ```
