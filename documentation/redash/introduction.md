---
layout: page
title: Redash
---

# Redash

## Pre-requisites
```
sudo yum -y install epel-release
sudo yum -y install pwgen gcc-c++ libffi-devel openssl-devel xmlsec1 xmlsec1-openssl freetds freetds-devel
```

## mysql-devel installation
```
cd /usr/local/src
wget https://dev.mysql.com/get/mysql57-community-release-el6-8.noarch.rpm
yum -y localinstall mysql57-community-release-el6-8.noarch.rpm

yum repolist

vim /etc/yum.repos.d/mysql-community.repo
make below changes
# Enable to use MySQL 5.6
enabled=1
# Enable to use MySQL 5.7
enabled=0

yum -y install mysql-devel
```

## Add user for redash
```
useradd redash -s /sbin/nologin
```

## Redis installation
[Redis Installation](/documentation/redis/)

## Postgresql installation
[Postgresql Installation](/documentation/postgresql/) . Keep /var/lib/pgsql/9.5/data/pg_hba.conf as-is during installation. Don't change postgres related rules.

## Supervisor installation
[Supervisor Installation](/documentation/supervisor/)

## Directory setup
```
mkdir -p /opt/redash/
chown redash /opt/redash/

sudo -u redash wget https://raw.githubusercontent.com/getredash/redash/master/setup/ubuntu/files/env -O /opt/redash/.env

# generate cookie secret
pwgen -1s 32
# add in /opt/redash/.env
export REDASH_COOKIE_SECRET=secret_key
```

## Code setup
```
sudo -u redash wget https://s3.amazonaws.com/redash-releases/redash.1.0.1.b2833.tar.gz -O /tmp/redash.tar.gz
sudo -u redash mkdir /opt/redash/redash.1.0.1.b2833
sudo -u redash tar -C /opt/redash/redash.1.0.1.b2833 -xvf /tmp/redash.tar.gz
ln -nfs /opt/redash/redash.1.0.1.b2833 /opt/redash/current
ln -nfs /opt/redash/.env /opt/redash/current/.env

cd /opt/redash/
virtualenv-2.7 --no-site-packages .

source bin/activate
pip install --upgrade pip
pip install setproctitle # setproctitle is used by Celery for "pretty" process titles
pip install -r /opt/redash/current/requirements.txt
pip install -r /opt/redash/current/requirements_all_ds.txt
deactivate
```

## Database configuration
```
# Create user and database
sudo -u postgres createuser redash --no-superuser --no-createdb --no-createrole
sudo -u postgres createdb redash --owner=redash

cd /opt/redash/current

sudo -u redash bash
cd /opt/redash/current
source ../bin/activate
bin/run ./manage.py database create_tables
deactivate
exit
```

## Supervisor conf
```
# directory for gunicorn log files
mkdir -p /var/log/gunicorn/
wget -O /etc/supervisor/conf.d/redash.conf https://raw.githubusercontent.com/getredash/redash/master/setup/ubuntu/files/supervisord.conf

# add below changes in redash.conf
1. change path for gunicorn and celery to /opt/redash/bin/gunicorn and /opt/redash/bin/celery
2. add log file path in each of the 3 processes as:
stdout_logfile = /var/log/gunicorn/process_name-std.log
stderr_logfile = /var/log/gunicorn/process_name-err.log

supervisorctl restart
```

## reverse proxy setup with apache (https)
```
sudo vim /etc/httpd/conf.d/redash.conf
<VirtualHost *:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key
    ServerName redash.server.com
    ProxyPass / http://127.0.0.1:5000/ retry=0
    ProxyPassReverse / http://127.0.0.1:5000/
</VirtualHost>
```
