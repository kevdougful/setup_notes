# minidlna

    sudo apt-get install minidlna
    
stop the service

    sudo service minidlna stop

edit the config file

    sudo nano /etc/minidlna.conf

change the following settings

```
...
media_dir=A,/media/media01/Audio
media_dir=P,/media/media01/Picture
media_dir=V,/media/media01/Video
...
merge_media_dir=yes
...
friendly_name=MochaMedia
...
inotify=yes
```
