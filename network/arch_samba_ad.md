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
2. Install Samba 4  
  `pacman -Sy samba4`
3. Install NTP for use as a time server (optional)  
  `pacman -Sy ntp`
4. Install cups print server package (optional)
  `pacman -Sy cups`

