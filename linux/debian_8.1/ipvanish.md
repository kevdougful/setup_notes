# IPVanish VPN setup

Download files:
```
cd /tmp
wget http://www.ipvanish.com/software/configs/ca.ipvanish.com.crt
wget http://www.ipvanish.com/software/configs/ipvanish-US-New-York-nyc-a01.ovpn 
```
Install software:
```
sudo apt-get install network-manager-openvpn 
```
Create credential file
```
$ sudo touch /etc/openvpn/ipvanish_creds
$ sudo chmod 600 /etc/openvpn/ipvanish_creds
```
Copy username and password into file (each on their own line)
Move OpenVPN config files:
```
$ sudo cp /tmp/ipvanish-US-New-York-nyc-a01.ovpn /etc/openvpn/ipvanish-US-New-York-nyc-a01.conf
$ sudo cp /tmp/ca.ipvanish.com.crt /etc/openvpn/ca.ipvanish.com.crt
```
The conf file should look as follows: (get it pointed at the right files)
```
client
dev tun
proto udp
remote nyc-a01.ipvanish.com 443
resolv-retry infinite
nobind
persist-key
persist-tun
persist-remote-ip
ca /etc/openvpn/ca.ipvanish.com.crt
tls-remote nyc-a01.ipvanish.com
auth-user-pass /etc/openvpn/ipvanish_creds
comp-lzo
verb 3
auth SHA256
cipher AES-256-CBC
keysize 256
tls-cipher DHE-RSA-AES256-SHA:DHE-DSS-AES256-SHA:AES256-SHA
```
Start up the OpenVPN service
```
$ sudo service openvpn start
```
