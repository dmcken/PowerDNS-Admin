# Setup MySQL database for PowerDNS-Admin

This guide will show you how to prepare a MySQL or MariaDB database for PowerDNS-Admin.

We assume the database is installed per your platform's directions (apt, yum, etc).

## Setup database:

Connect to the database (Usually using `mysql -u root -p` - then enter your MySQL/MariaDB root users password if applicable), then enter the following:
```
CREATE DATABASE `powerdnsadmin` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON `powerdnsadmin`.* TO 'pdnsadminuser'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD_HERE';
FLUSH PRIVILEGES;
quit
```
- If your database server is located on a different machine then change 'localhost' to '%'
- Replace YOUR_PASSWORD_HERE with a secure password.

## Install required packages:
### Red-hat based systems:
```
yum install MariaDB-shared mariadb-devel mysql-community-devel
```

If you use MariaDB ( from [MariaDB repositories](https://mariadb.com/resources/blog/installing-mariadb-10-on-centos-7-rhel-7/) )

### Debian based systems:
```
apt install libmysqlclient-dev
```

### Install python packages:
```
pip3 install mysqlclient==2.0.1
```


## Known issues:

Problem: If you plan to manage large zones, you may encounter some issues while applying changes. This is due to PowerDNS-Admin trying to insert the entire modified zone into the column history.detail.

Using MySQL/MariaDB, this column is created by default as TEXT and thus limited to 65,535 characters.

Solution: Convert the column to MEDIUMTEXT:
```
USE powerdnsadmin;
ALTER TABLE history MODIFY detail MEDIUMTEXT;
```
