[LINK](http://software.opensuse.org/download/package?project=isv:ownCloud:community&package=owncloud)

```
wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_8.0/Release.key
apt-key add - < Release.key

echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_8.0/ /' >> /etc/apt/sources.list.d/owncloud.list 
apt-get update
apt-get install owncloud
```
