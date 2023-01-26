# Installation of Icinga2 HA Cluster, IDO-Database, Icingaweb2 with Icinga Director and Grafana with InfluxDB2
## Overview:
- Installation of Icinga2 HA Cluster
- Installation of MySQL-Server and Icingaweb2
- Installation of Icinga Director
- Installation of InfluxDB2 and Grafana
## Server

| IP | Servername | Services |
|---|---|---|
| 10.20.208.2 | srvicingaweb01 | Icingaweb2 + DB |
| 10.20.208.3 | srvgrafana01 | Grafana + InfluxDB2 |
| 10.20.208.4 | srvicinga01 | Icinga Master 1 |
| 10.20.208.5 | srvicinga02 | Icinga Master 2 |

## Install Icinga2 HA Cluster
Source: https://icinga.com/docs/icinga-2/latest/doc/02-installation/02-Ubuntu/
### Add sources
```bash
apt-get update
apt-get -y install apt-transport-https wget gnupg

wget -O - https://packages.icinga.com/icinga.key | apt-key add -

. /etc/os-release; if [ ! -z ${UBUNTU_CODENAME+x} ]; then DIST="${UBUNTU_CODENAME}"; else DIST="$(lsb_release -c| awk '{print $2}')"; fi; \
 echo "deb https://packages.icinga.com/ubuntu icinga-${DIST} main" > \
 /etc/apt/sources.list.d/${DIST}-icinga.list
 echo "deb-src https://packages.icinga.com/ubuntu icinga-${DIST} main" >> \
 /etc/apt/sources.list.d/${DIST}-icinga.list

apt-get update
```
### Install Icinga2
```bash
apt-get install icinga2
```

### Install Check Plugins
```bash
apt-get install monitoring-plugins
```

### Configure API
Run the following command to:
* enable the ```api``` feature,
* set up certificates, and
add the API user ```root``` with an auto-generated password in the configuration file ```/etc/icinga2/conf.d/api-users.conf```
* add
```bash
icinga2 api setup
```
### HA Setup
* https://icinga.com/blog/2020/10/01/how-to-set-up-high-availability-masters/
* https://icinga.com/docs/icinga-2/latest/doc/06-distributed-monitoring/

## Install Icingaweb2 Server
### Install MySQL-Server
### Install Icingaweb2
### Configure Icingaweb2
### Install Director
### Configure Director