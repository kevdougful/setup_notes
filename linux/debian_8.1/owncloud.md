[LINK](http://software.opensuse.org/download/package?project=isv:ownCloud:community&package=owncloud)

```
wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_8.0/Release.key
apt-key add - < Release.key

echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_8.0/ /' >> /etc/apt/sources.list.d/owncloud.list 
apt-get update
apt-get install owncloud
```
I set it up using SQLite at first on accident

so I switched it to MySQL using the instructions found [here](https://doc.owncloud.org/server/7.0/admin_manual/maintenance/convert_db.html):

Keep in mind that you need to run the occ command as the www-data user (or whatever user the web server uses)

The new database needs to be created first

```
mysql -u root -p
CREATE DATABASE owncloud_db
```
Then run the occ command
```
sudo -u www-data php occ db:convert-type --password <mysql pw> mysql oc_mysql 127.0.0.1 owncloud_db
```




Connect to: `<server IP>/owncloud`
