## HomeWork 5
### Попытайтесь запустить кластер - sudo -u postgres pg_ctlcluster 15 main start
### Напишите получилось или нет и почему
  Возникает ошибка так как в файле `postgresql.conf` в параметре `data_directory` указан путь к старому кластеру
  а не к находящемуся на новом диске
### Попытайтесь запустить кластер - sudo -u postgres pg_ctlcluster 15 main start
  Сервер постгрес выдает ошибку связанную с PID остановленного кластера
  решил м помощью `/usr/lib/postgresql/15/bin/pg_ctl restart -D /mnt/data/15/main`
### Зайдите через через psql и проверьте содержимое ранее созданной таблицы
  Содержимое таблицы на месте
### задание со звездочкой *
sudo mount -o defaults /dev/vdb1 /mnt/data
