# Введение в MySQL ч.1

MySQL - очень популярная система управления базами данных (СУБД). Изучение MySQL - очень необходимо для программистов. Особенно это нужно для веб-программирования, которое почти всегда использует MySQL в качестве базы данных.

## Создание базы данных MySQL.

В MySQL (а также в других приложениях реляционных баз данных) база данных представляет собой набор взаимосвязанных таблиц. База данных - это место, где будут создаваться таблицы.

Команда для создания базы данных: CREATE DATABASE [IF NOT EXISTS] db_name.

```sql
mysql> CREATE DATABASE IF NOT EXISTS my_db;
Query OK, 1 row affected (0.00 sec)

mysql>
```

## Просмотр списка баз данных MySQL

Чтобы просмотреть список всех баз данных в MySQL Server, используйте запрос:

```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| TEST_DB_1          |
| app1               |
| my_db              |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
7 rows in set (0.00 sec)

mysql>
```

## Выбор базы данных.

Для того чтобы выбрать базу данных в MySQL используйте следующюю команду: USE db_name;

```sql
mysql> USE my_db;
Database changed
mysql>

```

## Создание таблицы MySQL.

Перед тем как начать создавать таблицы, необходимо создать базу данных и выбрать её:

```sql
mysql> CREATE DATABASE IF NOT EXISTS my_db;
Query OK, 1 row affected (0.00 sec)

mysql> USE my_db;
Database changed
mysql>
```

Теперь можно создать таблицу:

```sql
mysql> CREATE TABLE users(
    -> name varchar(100),
    -> email varchar (100),
    -> address varchar (100),
    -> phone varchar (100));
Query OK, 0 rows affected (0.01 sec)

```

## Удаление базы данных MySQL.

В случае если вам по каким-то причинам больше не нужна база данных, вы можете её удалить (включая все таблицы и данные) с помощью данной команды: DROP DATABASE [IF EXISTS] db_name;

```sql
mysql> DROP DATABASE IF EXISTS my_db;
Query OK, 0 rows affected (0.01 sec)

mysql>
```

## Просмотр всех таблиц в MySQL.

Чтобы вывести список всех таблиц в текущей активной базе данных введите : SHOW TABLES;

```sql
mysql> SHOW TABLES;
+-----------------+
| Tables_in_my_db |
+-----------------+
| users           |
+-----------------+
1 row in set (0.00 sec)

mysql>
```

## Просмотр структуры таблицы в MySQL.

```sql
mysql> desc users;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| name    | varchar(100) | YES  |     | NULL    |       |
| email   | varchar(100) | YES  |     | NULL    |       |
| address | varchar(100) | YES  |     | NULL    |       |
| phone   | varchar(100) | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
4 rows in set (0.01 sec)

mysql>

```

## Удаление таблицы MySQL.

Для удаления таблицы используется следующая команда: DROP TABLE [IF EXISTS] table_name [, table_name, ...]

```sql
mysql> DROP TABLE IF EXISTS users;
Query OK, 0 rows affected (0.01 sec)

mysql>
```

## Изменение типа колонки.

Для изменения типа колонки используется запросы ALTER, MODIFY:

```sql
mysql> ALTER TABLE users MODIFY phone TEXT;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc users;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| name    | varchar(100) | YES  |     | NULL    |       |
| email   | varchar(100) | YES  |     | NULL    |       |
| address | varchar(100) | YES  |     | NULL    |       |
| phone   | text         | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql>
```

## Переименование столбца.

Для того чтобы переименовать столбец в MySQL нужно применить команды ALTER ... CHANGE:
ALTER TABLE tbl-name CHANGE old-name new-name type-data

```sql
mysql> ALTER TABLE users CHANGE phone phones varchar (100);
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

## Переименование таблицы.

Иногда нужно переименовать таблицу для этого MySQL предоставляет запрос ALTER… RENAME TO с форматом запроса:
ALTER TABLE tbl_name old RENAME TO new tbl-name;

```sql
mysql> ALTER TABLE users RENAME TO tbl_users;
Query OK, 0 rows affected (0.01 sec)
```

## Добавление нового столбца в таблицу.

С помощью запроса ALTER... ADD вы можете добавить новый столбец в таблицу :
ALTER TABLE users ADD column-name column-type;

```sql
mysql> ALTER TABLE users ADD is_admin TINYINT (1) NOT NULL DEFAULT 0;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql>

mysql> desc users;
+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| name     | varchar(100) | YES  |     | NULL    |       |
| email    | varchar(100) | YES  |     | NULL    |       |
| address  | varchar(100) | YES  |     | NULL    |       |
| phones   | varchar(100) | YES  |     | NULL    |       |
| is_admin | tinyint(1)   | NO   |     | 0       |       |
+----------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

mysql>
```

## Удаление столбца из таблицы.

Для удаления столба используется запрос ALTER… DROP:
ALTER TABLE tbl_name DROP col_name;

```sql
mysql> desc users;
+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| name     | varchar(100) | YES  |     | NULL    |       |
| email    | varchar(100) | YES  |     | NULL    |       |
| address  | varchar(100) | YES  |     | NULL    |       |
| phones   | varchar(100) | YES  |     | NULL    |       |
| is_admin | tinyint(1)   | NO   |     | 0       |       |
+----------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

mysql> ALTER TABLE users DROP is_admin;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc users;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| name    | varchar(100) | YES  |     | NULL    |       |
| email   | varchar(100) | YES  |     | NULL    |       |
| address | varchar(100) | YES  |     | NULL    |       |
| phones  | varchar(100) | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql>

```
