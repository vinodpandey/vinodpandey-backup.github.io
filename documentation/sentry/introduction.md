---
layout: page
title: Sentry
---
# Sentry

# Pre-requisites
## Redis
```
cd ~
sudo yum install -y tcl
wget http://download.redis.io/releases/redis-3.2.6.tar.gz
tar xzf redis-3.2.6.tar.gz
cd redis-3.2.6
make test
sudo make install
cd utils

# redis executable path - asked in below script - /usr/local/bin/redis-server
sudo ./install_server.sh 

# starting redis
sudo service redis_6379 start

# redis info
redis-cli info

# testing
redis-cli ping
PONG

## configuring redis

sudo vim /etc/sysctl.conf
vm.overcommit_memory=1
sudo sysctl vm.overcommit_memory=1
sudo sysctl -w fs.file-max=100000

# start redis on system restart
sudo chkconfig --add redis_6379
```

## PostgreSQL
```
rpm -Uvh http://yum.postgresql.org/9.5/redhat/rhel-6-x86_64/pgdg-redhat95-9.5-2.noarch.rpm

yum install postgresql95-server postgresql95 libpqxx-devel postgresql-devel

# initialize data
sudo service postgresql-9.5 initdb

# starting server
service  postgresql-9.5 start

# autostart on server restart
chkconfig postgresql-9.5 on

# psql setup
mv /usr/bin/psql /usr/bin/psql-bk
ln -sfn /usr/pgsql-9.5/bin/psql /usr/bin/psql

# commandline postgres
sudo -u postgres psql
postgres=#


sudo vim /var/lib/pgsql/9.5/data/pg_hba.conf
change peer and ident to md5

service  postgresql-9.5 start

```

# Installing via Python
## Dependencies
```
sudo yum install python-setuptools python-devel libxslt-devel gcc gcc-c++ libffi-devel libjpeg-devel libxml2-devel libxslt-devel libyaml-devel 
```

## Installation
```
cd /opt/
virtualenv-2.7 --no-site-packages sentry
cd sentry
source bin/activate
pip install sentry==8.15.0

sentry init /etc/sentry

/etc/sentry/sentry.conf.py

# Running Migrations
## create database and user
sudo -u postgres psql template1
template1=# CREATE DATABASE sentry;
template1=# CREATE USER sentry WITH PASSWORD 'sentry';
template1=# GRANT ALL PRIVILEGES ON DATABASE sentry to sentry;
template1=# \q

## verify user/password is working correctly
psql -h localhost -U sentry

update /etc/sentry/sentry.conf.py with database configuration (database, user, password, hostname)
add below line in /etc/sentry/sentry.conf.py to disable uesr registration
SENTRY_FEATURES["auth:register"] = False

## in sentry virtualenv
SENTRY_CONF=/etc/sentry sentry upgrade
SENTRY_CONF=/etc/sentry sentry createuser


## supervisor config for running sentry

[program:sentry-web]
directory=/opt/sentry/
environment=SENTRY_CONF="/etc/sentry"
command=/opt/sentry/bin/sentry start
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/gunicorn/sentry-web-std.log
stderr_logfile=/var/log/gunicorn/sentry-web-err.log

[program:sentry-worker]
directory=/opt/sentry/
environment=SENTRY_CONF="/etc/sentry"
command=/opt/sentry/bin/sentry run worker
user=centos
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/gunicorn/sentry-worker-std.log
stderr_logfile=/var/log/gunicorn/sentry-worker-err.log

[program:sentry-cron]
directory=/opt/sentry/
environment=SENTRY_CONF="/etc/sentry"
command=/opt/sentry/bin/sentry run cron
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/gunicorn/sentry-cron-std.log
stderr_logfile=/var/log/gunicorn/sentry-cron-err.log

```


## Old data cleanup 
sentry cleanup --days=30


