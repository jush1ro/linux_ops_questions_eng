1. How to set up master-slave replication in mysql (shortly)?

<details>
  <summary>Answer</summary>

2 servers are required: master and slave.

1. We install the same version of MySQL server on both servers.
2. We enable the database server on both servers.
3. We configure master - in `/etc/my.cnf` we set the following values:
```
# select server ID, any number, better to start with 1
server-id = 1
# binary file path
log_bin = /var/log/mysql/mysql-bin.log
# database name
binlog_do_db = newdatabase
```
Restart the database server.
4. We connect to the master server, create a user and assign him rights to perform replication.
```
mysql -u root -p <пароль root сервера БД>
GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
```
5. On the master server, we make a database dump with table locks.
```
mysqldump -u root -p --lock-all-tables newdatabase > newdatabase.sql
```
6. We transfer the database dump to the slave server, create a database with the same name and import the database.
```
CREATE DATABASE newdatabase;
mysql -u root -p newdatabase < newdatabase.sql
```
7. Setup slave в `/etc/my.cnf`:
```
# Slave ID, convenient to select the next number after the Master
server-id = 2
# Path relay log
relay-log = /var/log/mysql/mysql-relay-bin.log
# Path bin master log
log_bin = /var/log/mysql/mysql-bin.log
# database
binlog_do_db = newdatabase
```
Restart the database server.
8. We start replication on the slave server.
```
CHANGE MASTER TO MASTER_HOST='10.10.0.1', MASTER_USER='slave_user', MASTER_PASSWORD='password',
MASTER_LOG_FILE = 'mysql-bin.000001', MASTER_LOG_POS = 107;
##We take the specified values ​​from the Master settings.
After this, we start replication on the Slave:
START SLAVE;
```
9. Checking the replication status:
```
SHOW SLAVE STATUSG
```

</details>

2. What type of database does Prometheus use?

<details>
  <summary>Answe</summary>

Prometheus use TSDB (time series database).

</details>

3. What is VACUUM in PostgreSQL?

<details>
  <summary>Answer</summary>

VACUUM reclaims space occupied by "dead" tuples. In normal PostgreSQL operations, tuples deleted or obsoleted by an update are not physically removed from the table; they remain there until a VACUUM is issued. Therefore, it is necessary to periodically run VACUUM, especially for frequently modified tables.

Without a parameter, VACUUM processes all tables in the current database that the current user can vacate. If a table name is passed as a parameter, VACUUM processes only that table.

A simple VACUUM (without FULL) only frees space and makes it available for reuse. This form of the command can run concurrently with normal table read and write operations, since it does not require an exclusive lock. However, the freed space is not returned to the operating system (in most cases); it is simply left available for data in the same table. VACUUM FULL rewrites the entire contents of a table to a new disk file that contains nothing extra, allowing unused space to be returned to the operating system. This form is much slower and requires an exclusive lock for each table processed.

</details>