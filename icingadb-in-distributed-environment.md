# IcingaDB in an distributed environment (Icinga2 HA Cluster)
## Servers
| IP | Servername | Services |
|---|---|---|
| 10.20.208.2 | srvicingaweb01 | Icingaweb2 + mySQL |
| 10.20.208.4 | srvicinga01 | Icinga Master 1 |
| 10.20.208.5 | srvicinga02 | Icinga Master 2 |

## Installation
### On srvicinga0x
```bash
apt  install icingadb icingadb-redis-server
```
### On srvicingaweb01
```bash
apt install icingadb-web
```

## Configuration
### On srvicingaweb01
Set up MySQL database for Icinga DB:
```mysql
# mysql -u root -p

CREATE DATABASE icingadb;
CREATE USER 'icingadb'@'localhost' IDENTIFIED BY 'CHANGEME';
GRANT ALL ON icingadb.* TO 'icingadb'@'localhost';
CREATE USER 'icingadb'@'10.20.208.4' IDENTIFIED BY 'CHANGEME';
GRANT ALL ON icingadb.* TO 'icingadb'@'10.20.208.4';
CREATE USER 'icingadb'@'10.20.208.5' IDENTIFIED BY 'CHANGEME';
GRANT ALL ON icingadb.* TO 'icingadb'@'10.20.208.4';
```
### On srvicinga0x
```bash
mysql -u icingadb -p icingadb </usr/share/icingadb/schema/mysql/schema.sql
````
Edit ```/etc/icingadb/config.yml```

```bash
# This is the configuration file for Icinga DB.

# Connection configuration for the database to which Icinga DB synchronizes monitoring data.
# This is also the database used in Icinga DB Web to view and work with the data.
# In high availability setups, all Icinga DB instances must write to the same database.
database:
  # Database type. Either 'mysql' for MySQL or 'pgsql' for PostgreSQL.
  # Defaults to 'mysql'.
#  type: mysql

  # Database host or absolute Unix socket path.
  host: 10.20.208.2

  # Database port. By default, the MySQL or PostgreSQL port, depending on the database type.
#  port:

  # Database name.
  database: icingadb

  # Database user.
  user: icingadb

  # Database password.
  password: CHANGEME

# Connection configuration for the Redis server where Icinga 2 writes its configuration, state and history items.
# This is the same connection as configured in the 'icingadb' feature of the corresponding Icinga 2 node.
# High availability setups require a dedicated Redis server per Icinga 2 node and
# therefore a dedicated Icinga DB instance that connects to it.
redis:
  # Redis host or absolute Unix socket path.
  host: localhost

  # Redis port.
  # Defaults to '6380' since the Redis server provided by the 'icingadb-redis' package listens on that port.
#  port: 6380

  # Redis password.
#  password:
```
