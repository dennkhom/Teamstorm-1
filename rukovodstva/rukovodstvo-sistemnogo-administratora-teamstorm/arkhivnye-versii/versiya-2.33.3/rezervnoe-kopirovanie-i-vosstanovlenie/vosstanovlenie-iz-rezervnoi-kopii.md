# Восстановление из резервной копии

Данный раздел описывает процедуру восстановления данных для кластера TeamStorm и Test IT в случае системного сбоя.

{% hint style="info" %}
Для восстановления работы кластера нужно установить "чистую" версию TeamStorm и Test IT

Перед восстановлением убедитесь, что cистема пустая и работает корректно
{% endhint %}

Восстановление данных из резервной состоит из двух шагов и выполняется в порядке, обратном созданию резервных копий.

1. Восстановление разделов docker-контейнеров.
2. Восстановление баз данных PostgreSQL из ранее созданных дампов.

## &#x20;Восстановление разделов docker-контейнеров

Для восстановления разделов контейнеров кластера нужно иметь привилегии пользователя`root`, так как по-умолчанию только `root` имеет доступ к этим разделам файловой структуры.&#x20;

1. Создайте временную директорию и перейдите в нее. Затем скопируйте сюда архив, содержащий разделы контейнеров докер:

````
```shell
root@server: /root/ $ mkdir /tmp/volumes_backup
root@server: /root/ $ cd /tmp/volumes_backup
root@server: /tmp/volumes_backup/ $ scp backup-server:/teamstorm/volumes_backup.tgz .
root@server: /tmp/volumes_backup/ $ tar -xzvf volumes_backup.tgz
testit_ssl-volume/
testit_minio-data-volume/
testit_avatars-minio-data-volume/
root@server: /tmp/volumes_backup/ $ rm -f volumes_backup.tgz
```
````

2. Перенесите директории в папку c разделами `docker`. По умолчанию это `/var/lib/docker/volumes/`

````
```shell
mv testit_ssl-volume testit_minio-data-volume testit_avatars-minio-data-volume \
                                                            /var/lib/docker/volumes/
```
````

{% hint style="info" %}
Пользователь `root` больше не нужен, остальные операции можно делать из-под "обычного" пользователя
{% endhint %}

### Восстановление баз данных PostgreSQL из дампов

Данная процедура описывает баз данных PostgreSQL из предварительно созданных дампов. Процедура автоматизирована и для запуска нужно выполнить следующие действия:

1. Перейдите в директорию `scripts`, которая находится внутри директории с артефактами TeamStorm:

````shell-session
```shell
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
````

2. Создайте здесь директорию `dumps`, сохраните туда архив и распакуйте его:

````
```shell
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
user@server: ~/teamstorm/scripts $ scp backup-server:/teamstorm/teamstorm_2023-09-20_09-42-00_backup.tgz ./dumps/
user@server: ~/teamstorm/scripts $ cd dumps
user@server: ~/teamstorm/scripts/dumps/ $ tar -xzvf teamstorm_2023-09-20_09-42-00_backup.tgz
...
user@server: ~/teamstorm/scripts $ cd ..
```
````

3. Запустите скрипт `db_restore.sh`, указав целевую директорию и общий суффикс, состоящий из временной метки и расширения, например, для файлов:

````
```shell
teamstorm_migrations_db_2023-09-20_09-42-00.bak
teamstorm_attachment_db_2023-09-20_09-42-00.bak
teamstormdb_2023-09-20_09-42-00.bak
...
```
````

Общим суффикcом будет `2023-09-20_09-42-00.bak`. Таким образом, команда для восстановления будет выглядеть следующим образом:

````
```shell
user@server: ~/teamstorm/scripts $ ./db_restore.sh ./dumps 2023-09-20_09-42-00.bak
```
````

{% hint style="warning" %}
Обращайте внимание на лог восстановления, при необходимости перенаправьте поток вывод в файл для дальнейшего анализа.
{% endhint %}

Пример успешного восстановления:

````
```shell
...
Recovering from backup: container: teamstorm-database_service-1 teamstormdb teamstormdb_15-09-2023_15-13.bak
UPDATE 1
pg_terminate_backend
----------------------
t
t
t
t
(4 rows)

DROP DATABASE
CREATE DATABASE
```
````

Пример неудачной попытки восстановления из несуществующего дампа.

````
```shell
Recovering from backup: container: teamstorm-database_service-1 teamstorm_migrations_db teamstorm_migrations_db_15-09-2023_15-13.bak
lstat /home/tester/deploy/teamstorm/scripts/dumps/teamstorm_migrations_db_15-09-2023_15-13.bak:
no such file or directory
UPDATE 1
pg_terminate_backend
----------------------
(0 rows)

DROP DATABASE
CREATE DATABASE
pg_restore: error: could not open input file "/teamstorm_migrations_db_15-09-2023_15-13.bak":
No such file or directory
```
````

4. Перезапустите кластер `teamstorm`:

````
```shell
docker compose -p teamstorm restart
```
````

Процесс по восстановлению для кластера TeamStorm успешно завершен.

5. Повторите те же шаги для кластера TestIt:

````
```shell
user@server: ~/teamstorm $ cd ../../testit/scripts
user@server: ~/testit/scripts $ mkdir dumps
user@server: ~/testit/scripts $ scp backup-server:/teamstorm/testit_2023-09-20_09-59-49_backup.tgz ./dumps/
user@server: ~/testit/scripts $ cd dumps
user@server: ~/testit/scripts/dumps/ $ tar -xzvf teamstorm_2023-09-20_09-42-00_backup.tgz
...
user@server: ~/testit/scripts $ cd ..
user@server: ~/testit/scripts $ ./db_restore.sh ./dumps 2023-09-20_09-59-49.bak
Recovering from backup: container:
...
```
````

6. Перезапустите кластер `testit`:

````
```shell
docker compose -p testit restart
```
````

Процесс по восстановлению для кластера Test IT успешно завершен.

Убедитесь в работоспособности приложения после восстановления.
