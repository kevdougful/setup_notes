# OwnCloud Setup

## Installation
[LINK](http://software.opensuse.org/download/package?project=isv:ownCloud:community&package=owncloud)

```
wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_8.0/Release.key
apt-key add - < Release.key

echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_8.0/ /' >> /etc/apt/sources.list.d/owncloud.list 
apt-get update
apt-get install owncloud
```
## Change Database
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
## Change Data Directory
[LINK](https://forum.owncloud.org/viewtopic.php?t=7118)  
  
1. Stop apache with `sudo service apache2 stop`
2. Check if config.php already contains a `datadirectory` entry.  If so, remember it (i.e. copy/paste it somewhere)
3. Change/create the `datadirectory` entry in config.php
4. Make sure the `datadirectory` you specified above does not exist.
5. `mv` all the existing file from the original directory to the new location
6. `chown` the ownership of all the files/dirs to `www-data`  
    `chown -R www-data:www-data <new dir>`
7. Start apache with `sudo service apache2 start`
8. You may have to rescan files.

## SSL Setup

[LINK](http://sharadchhetri.com/2013/05/24/how-to-configure-self-signed-ssl-certificate-in-owncloud-ubuntu/)  

1. Install OpenSSL  
    `sudo apt-get install openssl`
2. Enable SSL and Rewrite module in Apache 2  
    ```
    sudo su -
    a2enmod ssl
    a2enmod rewrite
    ```
3. Create `ssl` directory inside `/etc/apache2`  
    `mkdir -p /etc/apache2/ssl`
4. Create self signed SSL certificate.  
    `openssl req -new -x509 -days 365 -nodes -out /etc/apache2/ssl/owncloud.pem -keyout /etc/apache2/ssl/owncloud.key`
5. Edit `owncloud.conf`  
    ```
    nano /etc/apache2/conf-enabled/owncloud.conf
    ```
    ```xml
    <VirtualHost 192.168.1.34:80>
      RewriteEngine on
      ReWriteCond %{SERVER_PORT} !^443$
      RewriteRule ^/(.*) https://%{HTTP_HOST}/$1 [NC,R,L]
    </VirtualHost>
     
    <VirtualHost 192.168.1.34:443>
      SSLEngine on
      SSLCertificateFile /etc/apache2/ssl/owncloud.pem
      SSLCertificateKeyFile /etc/apache2/ssl/owncloud.key
      DocumentRoot /var/www/owncloud/
       
      <Directory /var/www/owncloud>
        AllowOverride All
        order allow,deny
        Allow from all
      </Directory>
    </VirtualHost>
    ```
6. Restart Apache  
    `service apache2 restart`  
Connect to: `<server IP>/owncloud`
