---
layout: page
title: Networking Tools
---

# Networking Tools

#### tcpdump
```sh
yum -y install tcpdump
tcpdump -n port mysql

# analyze last 100 data packets
tcpdump -ni eth0 -c 100
```
* https://danielmiessler.com/study/tcpdump/

#### netstat
```sh
netstat -lnp | grep mysql
netstat -na | grep -i list
netstat -tnlp
telnet localhost 3306
```