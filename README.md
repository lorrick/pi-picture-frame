# pi-picture-frame
A digital picture frame that uses feh and dynamically updates the slideshow based on tags in images


1) Install Raspian on a model 3 or better Pi.
1) Install feh - sudo apt update; sudo apt -y install feh




> 6 8 1,15 * * export DISPLAY=:0;/usr/local/bin/feh -FYNGzx -D 200 --image-bg black --scale-down --auto-rotate --geometry 1600x1140 -. /mnt/PictureMount &
> 
> 0 0 1,15 * * killall feh
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


pi@piframe:/mnt $ sudo mount -t cifs -o username=pi //lilnasx/PictureMount/ /mnt/PictureMount/
Password for pi@//lilnasx/PictureMount/:  ********


cat /etc/fstab
> //192.168.1.4/PictureMount	/mnt/PictureMount/	cifs	auto,x-systemd.automount,credentials=/etc/samba/credentials/lurch	0	 0
> 

[   65.557798] CIFS: Attempting to mount //lilnasx/PictureMount
[   65.603632] CIFS: Status code returned 0xc000006d STATUS_LOGON_FAILURE
[   65.603673] CIFS: VFS: \\lilnasx Send error in SessSetup = -13
[   65.603730] CIFS: VFS: cifs_mount failed w/return code = -13


[  243.697158] CIFS: Attempting to mount //lilnasx/PictureMount

# TrueNAS Setup
Add mount and pi user to TrueNas

$# mkdir -p /etc/samba/credentials
$# vi lilnasx
>
> root@piframe:/etc/samba/credentials# cat lilnasx
> username=pi
> password=<password here>
> 



# TrueNAS Script

Install exiv2
https://www.justinsilver.com/random/fix-pkg-on-freenas-11-2/

> pkg install -y exiv2

```
lorrick@Lurch:/volume1/bin$ cat pic-process.sh
BASEDIR=/tmp

before=`wc -l /tmp/index.txt | cut -f 1 -d "/"`
echo "This is before ${before}"
echo "cp /tmp/index.txt /tmp/index.before"
cp /tmp/index.txt /tmp/index.before

find /mnt/volume1/Pictures/ -print -type f -name "*.[Jj][Pp][gG]" -exec exiv2 -u -p x pr {} 2> /dev/null \; | egrep "volume1|Xmp.dc.subject" | grep -B 1 "Frame-done" | grep -v Frame-done |grep -v -- -- > /tmp/index.txt

#cat /tmp/index.txt
cp /tmp/index.txt /tmp/index.find

sed -i -e 's/^/ln -s "/g' /tmp/index.txt
sed -i -e 's/$/" \/mnt\/volume1\/PictureMount\//g' /tmp/index.txt
chmod 777 /tmp/index.txt
/tmp/index.txt

after=`wc -l /tmp/index.txt | cut -f 1 -d "/"`
echo "This is after ${after}"
count=`expr ${after} - ${before}`
echo "Done building PictureMount share. ${count} pictures added."
```







