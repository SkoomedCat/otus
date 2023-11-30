## Home Work 6 
### Логический уровень PostgreSQL
   1. создайте новую базу данных testdb
      
    create database testdb;
   2. зайдите в созданную базу данных под пользователем postgres
      
    \c testdb;
   3. создайте новую схему testnm
      
    create schema testnm;
   4. создайте новую таблицу t1 с одной колонкой c1 типа integer
      
    create table t1(c1 integer);
   5. вставьте строку со значением c1=1
       
    INSERT INTO t1(c1) VALUES (1);
   6. создайте новую роль readonly
       
    `CREATE ROLE readonly;`
   7. - дайте новой роли право на подключение к базе данных testdb
      - дайте новой роли право на использование схемы testnm
      - дайте новой роли право на select для всех таблиц схемы testnm
 
    GRANT CONNECT ON DATABASE testdb TO readonly;
    GRANT ALL ON SCHEMA testnm TO readonly;
    GRANT SELECT ON ALL TABLES IN SCHEMA testnm TO readonly;
 
   8. создайте пользователя testread с паролем test123
      
    CREATE USER testread;
    ALTER USER testread WITH PASSWORD 'test123';
    дайте роль readonly пользователю testread
    GRANT readonly TO testread;
    
   9. зайдите под пользователем testread в базу данных testdb
    `\c testdb testread`

   11. Так же сделайте `select * from t1;`
       Получилось?
       
    Нет, пишет permission denied for table t1
    так как таблица по умолчанию создалась в схеме public

   12. Создайте ее заново но уже с явным указанием имени схемы testnm
       вставьте строку со значением c1=1
       зайдите под пользователем testread в базу данных testdb
       сделайте select * from testnm.t1;
       ```
       DROP TABLE t1;
       CREATE TABLE testnm.t1(c1 integer);
       INSERT INTO testnm.t1 values(1);
   13. Получилось?
       
    Нет, потому что GRANT SELECT  ALL TABLES IN SCHEMA testnm TO readonly дал 
    доступ только для существующих на тот момент времени таблиц а t1 пересоздавалась
   14. Как сделать так чтобы такое больше не повторялось?
       
    ALTER DEFAULT PRIVILIGES IN SCHEMA testnm GRANT SELECT ON TABLES TO readonly;
    SELECT * FROM testnm.t1;
    
   15. Теперь попробуйте выполнить команду `CREATE TABLE t2(c1 integer); INSERT INTO t2 VALUES(2);`
   16. Есть идеи как убрать эти права?
       ```
       \c testdb postgres; 
       REVOKE CREATE on SCHEMA public FROM public; 
       REVOKE ALL on DATABASE testdb FROM public; 
       \c testdb testread;
       ```
     
   18. Расскажите что сделали и почему
       ```
       Это все потому что search_path указывает в первую очередь на схему public. 
       А схема public создается в каждой базе данных по умолчанию. 
       И grant на все действия в этой схеме дается роли public. 
       А роль public добавляется всем новым пользователям. 
       Соответсвенно каждый пользователь может по умолчанию создавать объекты в схеме public любой базы данных,
       ```
   19. 
       Теперь попробуйте выполнить команду `create table t2(c1 integer); insert into t2 values (2);`
       
       Ответ:
       
       `permission denied for schema public, доступа к схеме паблик уже нет`

