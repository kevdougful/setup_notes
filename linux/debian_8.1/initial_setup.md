# Debian 8.1 Install
## Installed from DVDs

__Installed:__
- ssh server
- Debian desktop
- scanned DVD 1-3 for apt

used [http://debgen.simplylinux.ch/](http://debgen.simplylinux.ch/} for sources.list generation

_/etc/apt/sources.list:_
```
deb http://ftp.us.debian.org/debian testing main contrib non-free
deb-src http://ftp.us.debian.org/debian testing main contrib non-free

deb http://ftp.debian.org/debian/ jessie-updates main contrib non-free
deb-src http://ftp.debian.org/debian/ jessie-updates main contrib non-free

deb http://security.debian.org/ jessie/updates main contrib non-free
deb-src http://security.debian.org/ jessie/updates main contrib non-free
```
Ran `apt-get update` and `apt-get install sudo` then added myself to the sudo group
```
usermod -a -G sudo kevin
```
Install git
```
sudo apt-get install git-core
```
Install xRDP
```
sudo apt-get install xrdp
```
