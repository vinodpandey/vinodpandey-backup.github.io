---
layout: page
title: MathTeX
---
# MathTeX

## Pre-requisites

```
wget -O texlive2017.iso http://mirrors.concertpass.com/tex-archive/systems/texlive/Images/texlive2017.iso

mkdir -p /mnt/iso

mount -t iso9660 -o loop texlive2017.iso /mnt/iso/

cd /mnt/iso
./install-tl

[... messages omitted ...]
Enter command: i
[... when done, see below for post-install ...]

cd ~
umount /mnt/iso

path: /usr/local/texlive/2017/bin/x86_64-linux

vim ~/.bash_profile
update path as below
PATH=/usr/local/texlive/2017/bin/x86_64-linux:$PATH

source ~/.bash_profile

which latex
which dvipng

latex -v
dvipng -v
```

## Installation

```
make sure apache is not running in worker mode (see installation issue below)

wget -O mathtex.zip http://www.forkosh.com/mathtex.zip
unzip mathtex.zip

cc mathtex.c -DLATEX=\"$(which latex)\" -DDVIPNG=\"$(which dvipng)\" -DPNG -o mathtex.cgi

move mathtex.cgi to /cgi-bin/
chmod +x mathtex.cgi
chown apache:apche mathtex.cgi

# with restricting access to specific domains
cc mathtex.c -DLATEX=\"$(which latex)\" -DDVIPNG=\"$(which dvipng)\" -DREFERER=\"mycbseguide.com,localhost.mycbseguide.com\" -DPNG -o mathtex.cgi

# testing commandline
 ./mathtex.cgi   "x^2+y^3"   -m 9   -o test

# testing web
http://website-url/cgi-bin/mathtex.cgi?x^2+y^2
```

## Installation issues

```
apache should not be running in worker mode
vim /etc/sysconfig/httpd
comment below line (if apache is running in worker mode) and restart httpd

HTTPD=/usr/sbin/httpd.worker
```

## References
- [http://www.forkosh.com/mathtex.html](http://www.forkosh.com/mathtex.html)
