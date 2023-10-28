# Резервное копирование

Данный раздел описывает процедуру сохранения данных кластера TeamStorm и Test IT на случай системных сбоев, отказа оборудования и непредвиденных ситуаций.

{% hint style="warning" %}
Для создания архива остановка кластера необязательна, но выполнение команд резервного копирования может негативно отразиться на работе пользователей
{% endhint %}

Резервное копирование данных состоит из двух шагов.

1. Создание дампов баз данных PostgreSQL.
2. Создание файлового архива необходимых разделов docker-контейнеров.

## Создание дампа баз данных PostgreSQL

Данная процедура описывает создание дампов баз данных PostgreSQL. Процедура автоматизирована и для запуска нужно выполнить следующие действия:

1. Перейти в директорию `scripts`, которая находится внутри директории с артефактами TeamStorm:&#x20;

```
user@server: ~/teamstorm $ cd scripts
    user@server: ~/teamstorm/scripts $ ls -lha
    total 28
    drwxrwxr-x 3 user user 4096 Sep 19 13:24 ./
    drwxrwxr-x 4 user user 4096 Sep 19 13:23 ../
    -rwxrwx--- 1 user user 1270 Sep 18 11:22 copy_backup.sh*
    -rwxrwx--- 1 user user 1275 Sep 18 11:22 db_backup.sh*
    -rwxrwx--- 1 user user 1652 Sep 18 11:22 db_restore.sh*
    -rw-rw-r-- 1 user user 1737 Sep 18 11:22 postgres-init.sql
```

2. Создайте директорию там, где будет сохранен архив, например:

```
user@server: ~/teamstorm/scripts $ mkdir dumps
user@server: ~/teamstorm/scripts $ ls -lha
total 28
drwxrwxr-x 3 user user 4096 Sep 19 13:24 ./
drwxrwxr-x 4 user user 4096 Sep 19 13:23 ../
-rwxrwx--- 1 user user 1270 Sep 18 11:22 copy_backup.sh*
-rwxrwx--- 1 user user 1275 Sep 18 11:22 db_backup.sh*
-rwxrwx--- 1 user user 1652 Sep 18 11:22 db_restore.sh*
drwxr-xr-x 2 user user 4096 Sep 19 13:26 dumps/
-rw-rw-r-- 1 user user 1737 Sep 18 11:22 postgres-init.sql
```

3. Запустите скрипт `db_backup.sh`, указав целевую директорию, например:

```
user@server: ~/teamstorm/scripts $ ./db_backup.sh ./dumps
Creating backup: teamstorm_migrations_db_2023-09-20_09-42-00.bak container: database_service database: teamstorm_migrations_db
Successfully copied 2.56kB to /home/tester/deploy/teamstorm/scripts/dumps/teamstorm_migrations_db_2023-09-20_09-42-00.bak
Creating backup: teamstorm_attachment_db_2023-09-20_09-42-00.bak container: database_service database: teamstorm_attachment_db
Successfully copied 9.22kB to /home/tester/deploy/teamstorm/scripts/dumps/teamstorm_attachment_db_2023-09-20_09-42-00.bak
Creating backup: teamstorm_comment_db_2023-09-20_09-42-00.bak container: database_service database: teamstorm_comment_db
Successfully copied 44kB to /home/tester/deploy/teamstorm/scripts/dumps/teamstorm_comment_db_2023-09-20_09-42-00.bak
Creating backup: teamstormdb_2023-09-20_09-42-00.bak container: database_service database: teamstormdb
Successfully copied 486kB to /home/tester/deploy/teamstorm/scripts/dumps/teamstormdb_2023-09-20_09-42-00.bak

```

В процессе выполнения вы увидите сообщения о создании архивов для каждой базы данных. По окончании будет создан конечный архив, сжатый с использованием aрхиватора gzip:

<pre><code><strong>./dumps/teamstorm_wiki_db_2023-09-20_09-42-00.bak
</strong><strong>./dumps/teamstorm_workflow_db_2023-09-20_09-42-00.bak 
</strong><strong>./dumps/teamstorm_workspace_db_2023-09-20_09-42-00.bak 
</strong><strong>./dumps/teamstormdb_2023-09-20_09-42-00.bak 
</strong><strong>./dumps/teamstorm_2023-09-20_09-42-00_backup.tgz
</strong></code></pre>

Процесс по созданию архива для кластера TeamStorm успешно завершен.

4. Повторите эти же шаги для кластера Test IT:

```
user@server: ~/teamstorm $ cd ../../testit/scripts
user@server: ~/testit/scripts $ mkdir dumps
user@server: ~/testit/scripts $ ./db_backup.sh ./dumps
Creating backup: container: backgrounddb database: backgrounddb
Successfully copied 57.9kB to /home/tester/deploy/testit/scripts/dumps/backgrounddb_2023-09-20_09-59-49.bak
Creating backup: container: avatars.db database: avatarsdb
...
./dumps/testit_2023-09-20_09-59-49_backup.tgz
```

## Создание файлового архива необходимых разделов docker-контейнеров

Для создания копий разделов контейнеров кластера нужно иметь привилегии пользователя  `root` , так как по-умолчанию только `root` имеет доступ к этим разделам файловой структуры.&#x20;

Перейдите в директорию, где хранятся разделы docker-контейнеров. По умолчанию это `/var/lib/docker/volumes/`

```

admin@server: ~/testit/scripts $ sudo -s
root@server: ~/testit/scripts $ cd /var/lib/docker/volumes/
root@server: /var/lib/docker/volumes/ $ ls -lha
drwx-----x 33 root root   4096 Sep 20 08:35 ./
drwx--x--- 13 root root   4096 Sep 20 08:35 ../
brw-------  1 root root 253, 0 Sep 20 08:35 backingFsBlockDev
-rw-------  1 root root 131072 Sep 20 08:35 metadata.db
drwx-----x  3 root root   4096 Sep 20 08:31 teamstorm_cwm-rabbit-volume/
drwx-----x  3 root root   4096 Sep 20 08:31 teamstorm_cwm-rabbitmq-certificates-volume/
drwx-----x  3 root root   4096 Sep 20 08:31 teamstorm_database-service-volume/
drwx-----x  3 root root   4096 Sep 20 08:31 teamstorm_ssl-volume/
drwx-----x  3 root root   4096 Sep 20 08:31 teamstorm_trusted-certificates-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_auth-cache-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_auth-cache-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_authdb-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_authdb-volume/
drwx-----x  3 root root   4096 Sep 20 08:37 testit_avatars-minio-data-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_avatars-minio-export-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_avatars-minio-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_avatars.db-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_avatars.db-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_backgrounddb-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_db-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_db-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_globalsearchdb-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_influx-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_influx-volume/
drwx-----x  3 root root   4096 Sep 20 08:35 testit_license-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_licensedb-volume/
drwx-----x  3 root root   4096 Sep 20 08:37 testit_minio-data-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_minio-export-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_minio-tls-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_rabbit-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_rabbitmq-certificates-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_rabbitmq-configuration-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_ssl-volume/
drwx-----x  3 root root   4096 Sep 20 08:34 testit_trusted-certificates-volume/
drwx-----x  3 root root   4096 Sep 20 08:35 testit_verification-volume/
 
```

Заархивируйте необходимые директории с помощью `tar`, например:

`root@server: /var/lib/docker/volumes/ $ tar -czvf volumes_backup.tgz / testit_ssl-volume / testit_minio-data-volume / testit_avatars-minio-data-volume /`

{% hint style="info" %}
В данном примере указаны разделы, содержащие все данные, которые находятся в хранилище S3 minio, а также сертификаты SSL, которые вы, возможно, использовали для настройки HTTPS соединения.&#x20;

Укажите другие разделы, если вы также их используете.
{% endhint %}

Финальным шагом должно быть сохранение трех файлов архива в ваше хранилище резервных копий. При необходимости, процедуру можно автоматизировать с помощью `crontab`.
