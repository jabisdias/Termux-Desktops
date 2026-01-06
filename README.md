## 🤚🍥 Setting Ubuntu chroot - Manual install <a name=ubuntu-chroot-manual></a>

1. Enter Android shell with root privileges: 
```
su
```

2. Create a directory at `/data/local/tmp` for chroot environment
```
mkdir /data/local/tmp/chrootUbuntu
cd /data/local/tmp/chrootUbuntu
```

3. Download Ubuntu 22.04 rootfs: 
```
wget https://github.com/jabisdias/Termux-Desktops/releases/tag/Ubuntu/ubuntu-base-22.04-base-arm64.tar.gz
```

4. Unzip the downloaded file and create some folders to mount the sdcard
```
tar xpvf ubuntu-base-22.04-base.arm64.tar.gz --numeric-owner

mkdir sdcard
mkdir dev/shm
```

5. Create a start script: 
```
cd ../
vi start_ubuntu.sh
```
Copy and paste the following: 
```
#!/bin/sh

#Path of DEBIAN rootfs
DEBIANPATH="/data/local/tmp/chrootDebian"

# Fix setuid issue
busybox mount -o remount,dev,suid /data

busybox mount --bind /dev $DEBIANPATH/dev
busybox mount --bind /sys $DEBIANPATH/sys
busybox mount --bind /proc $DEBIANPATH/proc
busybox mount -t devpts devpts $DEBIANPATH/dev/pts

# /dev/shm for Electron apps
mkdir $DEBIANPATH/dev/shm
busybox mount -t tmpfs -o size=256M tmpfs $DEBIANPATH/dev/shm

# Mount sdcard
mkdir $DEBIANPATH/sdcard
busybox mount --bind /sdcard $DEBIANPATH/sdcard

# chroot into DEBIAN
busybox chroot $DEBIANPATH /bin/su - root
```

6. Make the script executable and run it: 
```
chmod +x start_debian.sh
sh start_debian.sh
```

7. The prompt will change to `root@localhost`. If you need to return to Termux just write `exit`. Let's execute some fixes: 
```
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "127.0.0.1 localhost" > /etc/hosts
