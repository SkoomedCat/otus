## HomeWork 5
### Попытайтесь запустить кластер - `sudo -u postgres pg_ctlcluster 15 main start`
### Напишите получилось или нет и почему
  * Возникает ошибка так как в файле `postgresql.conf` в параметре `data_directory` указан путь к старому кластеру
  * а не к находящемуся на новом диске
### Попытайтесь запустить кластер - `sudo -u postgres pg_ctlcluster 15 main start`
  * Сервер постгрес выдает ошибку связанную с PID остановленного кластера
  * решил м помощью `/usr/lib/postgresql/15/bin/pg_ctl restart -D /mnt/data/15/main`
### Зайдите через через psql и проверьте содержимое ранее созданной таблицы
  * Содержимое таблицы на месте
### задание со звездочкой *
```
sudo mount -o defaults /dev/vdb1 /mnt/data
sudo systemctl stop postgresql@15-main
sudo rm -r /var/lib/postgresql/15
sudo nano /etc/postgresql/15/main/postgresql.conf
sudo systemctl start postgresql@15-main
```
* Произошло автоудаление старого pid файла поэтому сервер запустился без проблем, данные в таблице сохранились
