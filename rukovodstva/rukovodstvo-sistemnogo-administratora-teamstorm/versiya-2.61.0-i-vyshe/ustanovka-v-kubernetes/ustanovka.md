# Установка

{% hint style="warning" %}
Перед установкой еще раз убедитесь в корректности выполненных шагов по подготовке к старту кластера.
{% endhint %}

1.  Примените настройки, выполненные относительно Test IT при предварительной настройке, где `${TESTIT_NAMESPACE}` — имя пространства Kubernetes, в котором развёрнуто ПО Test IT:

    ```shell
    cd ~/testit_vX.XX # 
    # Установите приложения бэкенда.
    helm upgrade -n ${TESTIT_NAMESPACE} /
                 -f testit_frontend/values-override.yaml /
                 ./testit_frontend
    # Дождитесь начала работы всех модулей внешнего интерфейса
    watch -n 1 kubectl -n ${TESTIT_NAMESPACE} get pods -l app=frontend
    ```
2.  Распакуйте файлы приложения TeamStorm и перейдите в разархивированную директорию:

    ```shell
    unzip teamstorm_helm_v%Release%.tgz
    cd ./teamstorm_vX.XX # где _vX.XX - номер версии
    ```
3.  Для запуска кластера приложения TeamStorm выполните следующую команду, установив значение `${TEAMSTORM_NAMESPACE}`:

    ```shell
    helm install teamstorm -n ${TEAMSTORM_NAMESPACE} .
    ```
