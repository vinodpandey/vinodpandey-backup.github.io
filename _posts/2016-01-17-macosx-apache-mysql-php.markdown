---
layout: post
title: "MacOSX Apache MySQL PHP"
date: 2016-01-17 08:12
comments: true
categories: 
---
## Apache

````sh
httpd -v
Server version: Apache/2.4.10 (Unix)
Server built:   Jan  8 2015 20:48:33
````

#### Document root

````sh
/Library/WebServer/Documents/
````

#### starting/stopping server

```sh
sudo apachectl start/stop/restart
```

#### changing port

````sh
sudo vim /etc/apache2/httpd.conf
# modify Listen 80 to Listen 8888
# restart apache
sudo apachectl restart
# access http://localhost:8888/
````

## PHP

```` sh
php -v
PHP 5.5.20 (cli) (built: Feb 25 2015 23:30:53) 
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
````

#### Enabling php module in apache

````sh
sudo vim /etc/apache2/httpd.conf
# uncomment below line
LoadModule php5_module libexec/apache2/libphp5.so
sudo apachectl restart
````

## MySQL

Download MacOSX dmg file from below location and install http://dev.mysql.com/downloads/mysql/   
Note down the temporary password

#### Starting/stopping MySQL server
````sh
System Preferences > MySQL
````

#### set command shortcut
````sh
sudo ln -sfn /usr/local/mysql/bin/mysql /usr/local/bin/mysql
sudo ln -sfn /usr/local/mysql/bin/mysql_config /usr/local/bin/mysql_config
````

#### changing mysql passwod
````sh
mysql -uroot -p
UPDATE mysql.user SET Password=PASSWORD('password') WHERE User='root';
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
SET PASSWORD = PASSWORD('password');
FLUSH PRIVILEGES; 
quit
````


## phpMyAdmin

#### setup mysql 
````sh
sudo mkdir /var/mysql
sudo ln -s /tmp/mysql.sock /var/mysql/mysql.sock
````

Download phpMyAdmin from https://www.phpmyadmin.net/ 

Unzip and copy to /Library/WebServer/Documents/  
Access http://localhost:8888/phpmyadmin/setup/  
Click > New Server
Authentication Tab > Update "Password for config auth"  
Click > Apply
Access: http://localhost:8888/phpmyadmin  

