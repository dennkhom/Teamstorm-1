# Изменение ссылки на видеоролик

Чтобы заменить приветственный видеоролик:

1. Перейдите в папку, куда был распакован архив поставки TeamStorm.
2. Найдите в файле `web_app.json` в папке `configs` из поставки TeamStorm ссылку на видеоролик.&#x20;

```
teamstorm]$ cat ./configs/web_app.json 
{ 
   "External_link": ""https://www.youtube-nocookie.com/embed/qyaZT5Un3zA?si=XQSLZ_Sj_TCLvbIr"" 
}
```

3. Замените существующую ссылку своей и сохраните изменения.&#x20;
4. Перезапустите TeamStorm: `docker compose -p teamstorm up -d` (выполняется в случае, когда изменения проводятся на уже установленной системе).



