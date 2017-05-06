---
layout: page
title: PostgreSQL
---

# Installation
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
postgres=# \q

sudo vim /var/lib/pgsql/9.5/data/pg_hba.conf
# update file as below

# TYPE  DATABASE        USER            ADDRESS                 METHOD

local   all             postgres                                     peer
# IPv4 local connections:
host    all             postgres             127.0.0.1/32            ident
# # IPv6 local connections:
host    all             postgres             ::1/128                 ident

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5


service  postgresql-9.5 restart

```
