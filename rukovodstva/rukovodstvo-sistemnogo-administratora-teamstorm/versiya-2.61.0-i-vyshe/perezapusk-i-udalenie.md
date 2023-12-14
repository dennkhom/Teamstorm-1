# Перезапуск и удаление

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
