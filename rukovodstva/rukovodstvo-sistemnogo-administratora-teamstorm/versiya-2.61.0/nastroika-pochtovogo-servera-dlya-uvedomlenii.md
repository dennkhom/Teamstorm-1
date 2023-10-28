# Настройка почтового сервера для уведомлений

Используйте доверенные сертификаты для соединения с использованием самоподписных сертификатов. Для этого необходимо положить файл, содержащий сгенерированный сертификат в формате PEM, в `docker volume trusted-certificates-volume`.&#x20;

Например:

```
$ ls /var/lib/docker/volumes/teamstorm_trusted-certificates-volume/_data/
/var/lib/docker/volumes/teamstorm_trusted-certificates-volume/_data/host.pem
MAIL_SERVER_USE_CUSTOM_TRUSTED_CA_BUNDLE="true"
```

Внесите изменения в env-файл:

1. Укажите хост размещения TeamStorm: `CWM_FRONTEND_URL="https://teamstorm.contoso.com"`
2. Для использования полной проверки без исключений используйте значение `MAIL_SERVER_USE_CUSTOM_TRUSTED_CA_BUNDLE="false"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USE_CUSTOM_TRUSTED_CA_BUNDLE="false"`

3. При пустом MAIL\_SERVER\_HOST значении сервер не отправляет сообщения. Укажите хост почтового сервера. Например: `MAIL_SERVER_HOST="smtp.mail.io"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_HOST=""`

4. Укажите порт почтового сервера. Например: `MAIL_SERVER_PORT="25"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_PORT="1025"`

5. Укажите сервисный аккаунт обратный адрес которого будет указан в заголовке сообщения. Например: `MAIL_SERVER_FROM="deamon@mail.io"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_FROM="deamon@service.com"`

6. Укажите имя сервисного аккаунта для отображения . Например: `MAIL_SERVER_DISPLAY_NAME="Teamstorm Mail Notification"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_DISPLAY_NAME="Mail Deamon"`

7. Укажите имя пользователя для аутентификации на сервере SMTP/IMAP . Например: `MAIL_SERVER_USER_NAME="username"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USER_NAME=" "`

8. Укажите пароль для аутентификации. Например:  `MAIL_SERVER_PASSWORD="password"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_PASSWORD=""`

9. Для подключения к почтовому серверу через STARTTLS укажите: `MAIL_SERVER_USE_START_TLS="true"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USE_START_TLS="false"`

10. &#x20;Для подключения к почтовому серверу через SSL укажите: `MAIL_SERVER_USE_SSL="true"`

&#x20;       Значение по умолчанию: `MAIL_SERVER_USE_SSL="false"`

11. Укажите идентификатор часового пояса, например: `MAIL_SERVER_TZ="Europe/Moscow"`

&#x20;       По умолчанию установлено московское время: `MAIL_SERVER_TZ="Europe/Moscow"`

&#x20;       Таблица идентификаторов временных зон:  [https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List)

12. После внесения изменений перезапустите систему.&#x20;
