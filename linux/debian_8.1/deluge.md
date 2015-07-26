# Deluge BitTorrent Setup

### Setup a user for Deluge:
```
$ sudo adduser --disabled-password --system --home /home/deluge --group deluge

Adding system user `deluge' (UID 109) ...
Adding new group `deluge' (GID 116) ...
Adding new user `deluge' (UID 109) with group `deluge' ...
Creating home directory `/home/deluge' ...
```

### Download and install Deluge
```
$ sudo apt-get update && sudo apt-get install deluged deluge-webui
```

### Configure the daemon to run at boot

To start deluge and the webui at boot, we need to have an init.d script set-up to start it during the boot process. Luckily for us, someone else has already written an init.d script, all we need to do is configure it, and copy it into our init.d directory. The first thing we need to do is create the /etc/default/deluge-daemon file. this is what passes the configuration to the OS.
```
$ sudo nano /etc/default/deluge-daemon
```
Paste the following into the new file and save.
```
# Configuration for /etc/init.d/deluge-daemon
# The init.d script will only run if this variable non-empty.
DELUGED_USER="deluge"
# Should we run at startup?
RUN_AT_STARTUP="YES"
```
Create the init.d script:
```
$ sudo nano /etc/init.d/deluge-daemon
```
Paste in the contents of [deluge-daemon](deluge-daemon)
Now we need to make it executable, and update the boot process to reflect that we added it:
```
$ sudo chmod a+x /etc/init.d/deluge-daemon && sudo update-rc.d deluge-daemon defaults
```
From here, you should be able to reboot the server, and access the web interface from [hostnameofMediaServer]:8112.
default password is "deluge"

### Setup `deluge-console`
First we need to download the console version of deluge so that we can configure a few settings:
```
$ sudo apt-get install deluge-console
```
Once it has finished installing go ahead and boot up deluge-console by typing:
```
$ sudo -H -u deluge deluge-console
```
in the console, allow remote connections to the daemon:
```
config -s allow_remote True
```
and exit:
```
exit
```
now we need to go ahead and shutdown the daemon
```
$ sudo /etc/init.d/deluge-daemon stop
```
and add a username and password for the daemon to the auth file:
```
$ sudo chmod -R 777 /home/deluge && sudo -H -u deluge echo username:password:10 >> /home/deluge/.config/deluge/auth
```
now we can go ahead and start the daemon back up:
```
$ sudo /etc/init.d/deluge-daemon start
```
remember to disable classic mode on your client and connect to the server's daemon using the username/password you just setup.
