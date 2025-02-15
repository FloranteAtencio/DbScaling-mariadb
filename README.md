# Configuration

Disclaimer 
I've done my best to ensure clarity and accuracy in this content. Please feel free to point out any errors or areas for improvement.

Run the composer then acess the master maraidb create database and sample plus user for replication_user
Note:replication_user should exists only in master database instance 

`CREATE DATABASE football; \
 USE football; \
 CREATE TABLE players (name varchar(50) DEFAULT NULL,position varchar(50) DEFAULT NULL); \
 INSERT INTO players VALUES ('Lionel Messi','Forward'); \
 GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY '123'; #enter password \
 FLUSH PRIVILEGES; \`
 `show master status; \`

access slave database instance :

 `CHANGE MASTER TO 
   MASTER_HOST='mariadb-master',
   MASTER_USER='slave_user',
   MASTER_PASSWORD='123',
   MASTER_LOG_FILE=<'--master-bin.000002--'>, --replace it base on your master staus appear
   master_use_gtid=<--slave_pos , '01-02-05'-->, -- it can be manual gtid type
   MASTER_LOG_POS=<--1199-->; --replace it base on your master staus appear
START SLAVE;`


for  master and slave

`CREATE USER 'maxscale'@'%' IDENTIFIED BY 'maxscale';
GRANT SUPER, REPLICA MONITOR, REPLICATION CLIENT, REPLICATION SLAVE, SHOW DATABASES, EVENT, PROCESS, SLAVE MONITOR, READ_ONLY ADMIN ON *.* TO 'maxscale'@'%';
GRANT SELECT ON mysql.* TO 'maxscale'@'%';
FLUSH PRIVILEGES;`


Debug Command:

relay_master_log_file:master-bin.000002
exec_master_log_pos=1930
`SELECT BINLOG_GTID_POS('master-bin.000002', '1930');`
In order to find the  gtid-pos

`SELECT @@gtid_current_pos;`
`SELECT @@gtid_slave_pos;`
this command will do for gtid too.

`SHOW VARIABLES LIKE 'log_bin';`
make user log bin = on

`DROP USER 'maxscale'@'%';`
`SHOW GRANTS FOR 'maxscale'@'%';`
`SELECT user, host FROM mysql.user;`
commands that related to user

`tail -f /var/log/maxscale/maxscale.log`
check for logs





