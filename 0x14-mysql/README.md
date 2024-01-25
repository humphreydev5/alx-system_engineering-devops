0x14. MySQL
https://camo.githubusercontent.com/61e15a4c6d7f21e3105d292ba3ba6fe0128591b40acecb27056a03a0b5fd03f5/68747470733a2f2f7777772e73696d706c696c6561726e2e636f6d2f696365392f667265655f7265736f75726365735f61727469636c655f7468756d622f646966666572656e63655f6265747765656e5f73716c5f616e645f6d7973716c2e6a7067

The MySQL server provides a database management system with querying and connectivity capabilities, as well as the ability to have excellent data structure and integration with many different platforms.

Needed Knowledge
What is a primary-replica cluster

MySQL primary replica setup

Build a robust database backup strategy

mysqldump

Learning Objectives
What is the main role of a database
What is a database replica
What is the purpose of a database replica
Why database backups need to be stored in different physical locations
What operation should you regularly perform to make sure that your database backup strategy actually works
Project Requirements
Allowed editors: vi, vim, emacs
All your files will be interpreted on Ubuntu 16.04 LTS
All your files should end with a new line
A README.md file, at the root of the folder of the project, is mandatory
All your Bash script files must be executable
Your Bash script must pass Shellcheck (version 0.3.7-5~ubuntu16.04.1 via apt-get) without any error
The first line of all your Bash scripts should be exactly #!/usr/bin/env bash
The second line of all your Bash scripts should be a comment explaining what is the script doing
Installation Guide for mysql 5.7.*
First go to site dev.mysql.com and copy the PGP PUBLIC KEY just immediately under the Notice section to your clipboard.

Create a new file in your terminal with a .key extension and paste the PGP PUB KEY copied to clipboard.

Then do the following

$ sudo apt-key add name_of_file.key
OK

# adding it to the apt repo
$ sudo sh -c 'echo "deb http://repo.mysql.com/apt/ubuntu bionic mysql-5.7" >> /etc/apt/sources.list.d/mysql.list'

# updating the apt repo to add the url i added earlier
$ sudo apt-get update

# now check your available versions
$ sudo apt-cache policy mysql-server
mysql-server:
  Installed: (none)
  Candidate: 8.0.31-0ubuntu0.20.04.2
  Version table:
     8.0.31-0ubuntu0.20.04.2 500
        500 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages
     8.0.31-0ubuntu0.20.04.1 500
        500 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages
     8.0.19-0ubuntu5 500
        500 http://us-east-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 Packages
     5.7.40-1ubuntu18.04 500
        500 http://repo.mysql.com/apt/ubuntu bionic/mysql-5.7 amd64 Packages

# Now am installing mysql 5.7.*
$ sudo apt-get install -f mysql-client=5.7* mysql-community-server=5.7* mysql-server=5.7* -y
Project Task
Creating a user and Granting Priviledges in mysql
$ mysql -root -p
Password:	/* Type root password

mysql> CREATE USER 'holberton_user'@'localhost' IDENTIFIED BY 'projectcorrection280hbtn';

mysql> GRANT GRANT REPLICATION CLIENT ON *.* TO 'holberton_user'@'localhost';

mysql> FLUSH PRIVILEGES;
Creating Database, Tables and adding Data to the Tables
mysql> CREATE DATABASE db_name_;

-- To verify if db is created
mysql> SHOW DATABASES;

mysql> USE db_name;

mysql> CREATE TABLE table_name (
    -> col_1 data_type,
    -> col_2 data_type);
-- continue adding more coloums to your taste for me i just added two coloumns

mysql> INSERT INTO table_name VALUES (val_1, val_2);

-- Verify if data was added succesfully do
mysql> SELECT col_1, col_2 FROM tb_name;
Setting Up MySQL Replication
First create replication user and grant replication priviledge ( best practice).
mysql> CREATE USER 'replica_user'@'%' IDENTIFIED BY 'replica_user_pwd';

mysql> GRANT REPLICATION SLAVE ON *.* TO 'replica_user'@'%';

mysql> FLUSH PRIVILEGES;

-- to verify
mysql> SELECT user, Repl_slave_priv FROM mysql.user;

mysql> exit
Next up you go to the /etc/mysql/mysql.conf.d/mysqld.cnf and comment the bind address and then add this lines to it
# By default we only accept connections from localhost
# bind-address = 127.0.0.1
server-id = 1
log_bin = /var/log/mysql/mysql-bin.log
binlog_do_db = db_name
Then you enable incoming connection to port 3306 and restart mysql-server
$ sudo ufw allow from 'replica_server_ip' to any port 3306

$ sudo service mysql restart
Now log back in to mysql-server to lock db and prepare binary file for replication.
$ mysql -uroot -p
password:
mysql> 

mysql> FLUSH TABLES WITH READ LOCK;

mysql> SHOW MASTER STATUS;

+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000001 |      149 | db           |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
Take note of the binary log and the position, jot it down or you leave this window open and you open another window to continue

you then export the db from myql-server to local machine and then copy this db to replica machine
$ mysqldump -uroot -p db_name > export_db_name.sql

$ scp -i _idenetity_file_ export_db_name.sql user@machine_ip:location
Then ssh to replica machine ip_adress to import this tables to replica mysql-server
$ mysql -uroot -p 
password:


mysql> CREATE DATABASE db_name;

mysql>exit
bye

$ mysql -uroot p db_name < export_db_name.sql
password:

# Now edit the config file in /etc/mysql/mysql.conf.d/mysqld.cnf and then reload mysql-server

```bash

server-id = 2
log_bin = /var/log/mysql/mysql-bin.log
binlog_do_db = db_name_from_master_mysql-server
relay_log = /var/log/mysql/mysql-relay-bin.log

$ sudo service mysql restart
Login to mysql server in replica to configure replication
$ mysql -uroot -p
password:


mysql>
mysql> CHANGE MASTER TO
    -> MASTER_HOST='source_host_name',
    -> MASTER_USER='replication_user_name',
    -> MASTER_PASSWORD='replication_password',
    -> MASTER_LOG_FILE='recorded_log_file_name',
    -> MASTER_LOG_POS=recorded_log_position;

-- Then you start slave
mysql> START SLAVE;
That's it you've configured replication on mysql, do reach out for any further assistance
