# Полезные команды

Команды, которые могут помочь администратору установить программное обеспечение TeamStorn при помощи Docker.&#x20;

| Команда                                                         | Назначение                                                         |
| --------------------------------------------------------------- | ------------------------------------------------------------------ |
| `docker version`                                                | Версия docker                                                      |
| `docker compose version`                                        | Версия docker compose                                              |
| `docker compose ls`                                             | Посмотреть список запущенных проектов docker compose               |
| `docker compose -p ${PROJECT_NAME} up -d --remove-orphans`      | Запустить проект с удалением более неиспользуемых контейнеров      |
| `docker compose -p ${PROJECT_NAME} restart`                     | Перезапустить проект                                               |
| `docker compose -p ${PROJECT_NAME} logs ${SERVICE}`             | Посмотреть логи сервисе в заданном проекте                         |
| `docker compose -p ${PROJECT_NAME} down --volumes`              | Остановить проект и очистить разделы контейнеров                   |
| `docker compose -p ${PROJECT_NAME} exec -ti -u 0 ${SERVICE} sh` | Запустить в контейнере оболочку `sh` с правами `root` пользователя |
