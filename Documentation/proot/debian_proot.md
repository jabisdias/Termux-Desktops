```
pkg update
pkg install root-repo
pkg install x11-repo
pkg install termux-x11-nightly
```
```
exit
```

```
pkg install proot-distro
```
```
proot-distro install debian
```
```
proot-distro login debian
```
```
apt update -y
apt install sudo nano adduser -y
```
```
adduser jabisdias
```
```
nano /etc/sudoers
```
```
jabisdias ALL=(ALL:ALL) ALL
```
```
sudo whoami 
```
```
proot-distro login debian --user jabisdias
```
```
sudo apt install xfce4
```
