---
layout: page
title: Sentry
---
# Sentry

# Pre-requisites
## Redis
* [Redis installation guide](/documentation/redis/)

## PostgreSQL
* [PostgreSQL installation guide](/documentation/postgresql/)

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
