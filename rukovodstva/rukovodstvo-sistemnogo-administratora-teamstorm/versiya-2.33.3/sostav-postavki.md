# Состав поставки

Архив автономной установки содержит папки:

1. `teamstorm`, которая содержит:
   * `configs` — конфигурационные файлы и плагины;
   * `scripts` — папка со  скриптами для резервного копирования и восстановления;
   * `images.list` — архив с образами;
   * `.env` — конфигурационный файл, содержащий переменные, используемые для обращения к контейнерам TeamStorm;
   * `docker-compose.yml` — конфигурационный файл Docker Compose;
   * `setup.sh` — скрипт для упрощенного развертывания TeamStorm и Test IT;
   * `setup_teamstorm.sh` — скрипт для автоматического развертывания TeamStorm;
2. `testit` с соответствующим набором компонентов, необходимых для установки ПО Test IT.
