# pi-picture-frame
A digital picture frame that uses feh and dynamically updates the slideshow based on tags in images


1) Install Raspian on a model 3 or better Pi.
1) Install feh - sudo apt update; sudo apt -y install feh




> 6 8 1,15 * * export DISPLAY=:0;/usr/local/bin/feh -FYNGzx -D 200 --image-bg black --scale-down --auto-rotate --geometry 1600x1140 -. /mnt/PictureMount &
> 
> 0 0 1,15 * * killall feh
> 



May not be used. Need to confirm
cat /etc/fstab
> //192.168.1.4/PictureMount	/mnt/PictureMount/	cifs	auto,x-systemd.automount,credentials=/etc/samba/credentials/lurch	0	 0
> 


```pi@piframe:/mnt $ ping lilnasx
PING lilnasx (192.168.1.96) 56(84) bytes of data.
64 bytes from lilnasx (192.168.1.96): icmp_seq=1 ttl=63 time=2.04 ms
64 bytes from lilnasx (192.168.1.96): icmp_seq=2 ttl=63 time=2.16 ms
```


```
pi@piframe:/mnt $ cat /etc/hosts
127.0.0.1	localhost
::1		localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters

127.0.1.1	piframe
192.168.1.96	lilnasx

```






