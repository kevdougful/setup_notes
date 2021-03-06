# Raspberry Pi Arch Linux Samba 4 Domain Controller

## Prepare the SD card

1. If not formatted, format sd card (`umount` sd card first)
    `dd if=/dev/zero of=/dev/sdX bs=8192`
1. Partition SD Card
    `fdisk /dev/sdX`
1. Use these steps in `fdisk`
  1. Type __o__. This will clear out any partitions on the drive.
  1. Type __p__ to list partitions. There should be no partitions left.
  1. Type __n__, then __p__ for primary, __1__ for the first partition on the drive, press __ENTER__ to accept the default first sector, then type __+100M__ for the last sector.
  1. Type __t__, then __c__ to set the first partition to type W95 FAT32 (LBA).
  1. Type __n__, then __p__ for primary, __2__ for the second partition on the drive, and then press __ENTER__ twice to accept the default first and last sector.
  1. Write the partition table and exit by typing __w__.
1. Create and mount the FAT filesystem  
  `mkfs.vfat /dev/sdX1`  
  `mkdir boot`  
  `mount /dev/sdX1 boot`  
1. Create and mount the ext4 filesystem  
  `mkfs.ext4 /dev/sdX2`  
  `mkdir root`  
  `mount /dev/sdX1 root`  
1. Download and extract the root filesystem (as root, not via sudo)
  `wget http://archlinuxarm.org/os/ArchLinuxARM-rpi-latest.tar.gz`  
  `bsdtar -xpf ArchLinuxARM-rpi-latest.tar.gz -C root`  
  `sync`  
1. Move the boot files to the first partition
  `mv root/boot/* boot`  
1. Unmount the two partitions
  `umount boot root`  
1. Insert into Raspberry Pi, default login/pass: root/root

## Initial Setup

- Change root password with `passwd`
- Set the timezone: `timedatectl set-timezone America/Chicago`
- Set the hostname: `hostnamectl set-hostname expresso`

## Install Samba 4

1. Update existing packages  
  `pacman -Syu`
1. Install Samba 4  
  `pacman -Sy samba4`
1. Install NTP for use as a time server (optional)  
  `pacman -Sy ntp`
1. Install cups print server package (optional)  
  `pacman -Sy cups`

## Setup Samba as a Domain Controller
### Provisioning the domain
1. Create a new domain   
  `samba-tool domain provision --realm=lab.local --domain=LAB --adminpass '<admin password>' --server-role=dc`
1. Backup the existing Kerberos config and replace it with the one generated by Samba  
  `mv /etc/krb5.conf /etc/krb5.conf.bak`  
  `cp /var/lib/samba/private/krb5.conf /etc/`  

### Start up Samba
1. Start Samba  
  `systemctl start samba`  
1. Start Samba on boot  
  `systemctl enable samba`  
1. Start ntpd and cups services on boot  
  `systemctl enable ntpd cups`

### Configure DNS Service
1. Edit `/etc/resolv.conf`  

    ```
        domain coffwillhaus.com  
        nameserver 192.168.1.101
    ```

1. Set to static IP  
  `cp /etc/netctl/examples/ethernet-static /etc/netctl/`
  `nano /etc/netctl/examples/ethernet-static`

    ```
    Description='A basic static ethernet connection'
    Interface=eth0
    Connection=ethernet
    IP=static
    Address='192.168.1.101
    #Routes=('192.168.0.0/24 via 192.168.1.1')
    Gateway='192.168.1.254'
    DNS=('192.168.1.254')
    
    ## For IPv6 autoconfiguration
    #IP6=stateless
    
    ## For IPv6 static address configuration
    #IP6=static
    #Address6=('1234:5678:9abc:def::1/64' '1234:3456::123/96')
    #Routes6=('abcd::1234')
    #Gateway6='1234:0:123::abcd'
    ```

1. Enable `samba_dnsupdate` command, add the following to the `[global]` section of `/etc/samba/smb.conf`  
  `nsupdate command = /usr/sbin/samba_dnsupdate`  
1. Create `/etc/resolv.conf.tail`  
  `nano /etc/resolv.conf.tail`  
  
  ```
  # Samba configuration
  search coffwillhaus.com
  # If using IPv6, uncomment the following line
  #nameserver ::1
  nameserver 127.0.0.1
  ```
  
1. Set permissions  
  `chmod 644 /etc/resolv.conf.tail`  
1. Regenerate the new /etc/resolv.conf  
  `resolvconf -u`  
  
### Configure NTP Service
[Docs](https://wiki.archlinux.org/index.php/Samba_4_Active_Directory_domain_controller#NTPD)

1. Backup `/etc/ntp.conf`  
  `cp /etc/ntp.conf /etc/net.conf.bak`  
1. `nano /etc/ntp.conf`  

    ```
    # Associate to the public NTP pool servers
    server 0.pool.ntp.org
    server 1.pool.ntp.org
    server 2.pool.ntp.org
    
    # Location of drift file
    driftfile /var/lib/ntp/ntpd.drift
    
    # Location of the log file
    logfile /var/log/ntpd
    
    # Location of the update directory
    ntpsigndsocket /var/lib/samba/ntp_signd/
    
    # Restrictions
    restrict default kod limited nomodify notrap nopeer mssntp
    restrict 127.0.0.1
    restrict ::1
    restrict 0.pool.ntp.org mask 255.255.255.255 nomodify notrap nopeer noquery
    restrict 1.pool.ntp.org mask 255.255.255.255 nomodify notrap nopeer noquery
    restrict 2.pool.ntp.org mask 255.255.255.255 nomodify notrap nopeer noquery
    ```

1. Create the state directory and set permissions  
  `install -d /var/lib/samba/ntp_signd`  
  `chown root:ntp /var/lib/samba/ntp_signd`  
  `chmod 0750 /var/lib/samba/ntp_signd`  
1. Start `ntpd` service  
  `systemctl start ntpd`  
  `systemctl enable ntpd`

Mostly from [this link](http://blog.dabasinskas.net/installing-samba-4-domain-controller-on-raspberry-pi-running-archlinux-arm/)
