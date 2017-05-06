---
layout: page
title: Redis
---

# Redis
Open source, in-memory data structure store, used as a database, cache and message broker.

```
## redis (installing latest version as redis 2.4 has a hard-corded limit of handling multiple connections)
# update maximum number of file handles for a single process allowed to open from 1024 to 65535
# https://sensuapp.org/docs/latest/installation/install-redis-on-rhel-centos.html

cd /tmp/
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

# ulimit increase in first step above

# start redis on system restart
sudo chkconfig --add redis_6379
```
