# Шпаргалка по созданию резервной копии БД Postgres и передачи ее с/на удаленный сервер с последующим развертованием.

 
1. Создание дампа Базы данных:

> pg_dump <database_name> > <dump_file_name>  –Fc –U <superuser_name>
> pg_dump warehouse > warehouse.dump  –Fc –U postgres

 

2. Передача файлов.

2.1. На сервер:

> scp -v /Users/evgenijakovenko/www-dev/postgres/warehouse.dump test:/home/stalker/warehouse.dump

2.2. Из сервера:

> scp test:/home/stalker/warehouse_210322.dump /Users/evgenijakovenko/

 прим. возможно дополнительно понадобится поднастроить файл /etc/ssh/ssh_config ssh клиента.

 

3. Востановление копии БД

3.1. Удаляем существующую базу.

> dropdb –U postgres warehouse

3.2. Востанавливаем копию.

> createdb -T template0 warehouse -U warehouse 
> pg_restore -d warehouse warehouse_210322.dump -U warehouse
