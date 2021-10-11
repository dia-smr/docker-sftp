# Start
```sudo ansible-playbook -i inventory -l localhost sftp.yml  --ask-pass```
# Stop
```sudo ansible-playbook -i inventory -l localhost sftp.yml  --ask-pass -t sftp-stop```

# Пароль
Пароль генерируется автоматически через сам ansible, в конце работы ansible он будет выведен на экране.
Послк аовторного запуска плэйбука, пароль останется прежним, его изменение будет произведено в случае, если образ еще не создан на сервере или в имедже задан пустой пароль

# SHH ключи
Для доступа по ключу, в контейнер был примонтирован файл "~/.ssh/authorized_keys"

# Данные 
Данные хранятся на volume созданном при запуске плэйбука

