# Подготовка к установке

Для обновления TeamStorm до версии 2.33.0 необходимо предварительно обновить TeamStorm до версии 2.0.0.

Перед обновлением рекомендуется создать резервную копию TeamStorm и Test IT и проверить соответствие наименований проектов в `docker-compose`. Возможна потеря данных при неправильном обновлении.

Для проверки выполнить:

```
$ docker compose ls 
NAME               STATUS             CONFIG FILES 
teamstorm          running(20)        ./deploy/offline_build/teamstorm/docker-compose.yml 
testit             running(20)        ./deploy/offline_build/testit/docker-compose.yml
```

## Настройка сервера для установки кластера TeamStorm

1.  Задайте параметры `vm.max_map_count=262144` и `vm.overcommit_memory=1`:

    ```bash
    echo 'vm.max_map_count=262144' >> /etc/sysctl.conf
    echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
    sysctl -p
    ```
2. Заблокируйте все порты, кроме порта 80, необходимого для доступа к пользовательскому интерфейсу.
3.  **Опционально:** для обслуживания системы посредством протокола SSH необходимо открыть порт 22 (может быть переназначено на конкретной конфигурации). Для работы по HTTPS необходимо открыть порт 443. Пример открытия доступа к портам для CentOS 8:

    ```bash
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    firewall-cmd --zone=public --add-port=22/tcp --permanent
    firewall-cmd --zone=public --add-port=443/tcp --permanent
    firewall-cmd --reload
    ```
