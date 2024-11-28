# Install and Configuration file
First, we are going to install the service that we will work with.
```
apt update
apt upgrade
apt install bind9
```
Later on, when we work with this service, we'll have to restart it, and maybe, if you have something wrong on the files, we'll have to use ```journalctl -u``` and ```systemctl bind9```
The command systemctl can work like this.
```
systemctl status
systemctl stop
systemctl restart
systemctl enable
systemctl disable
systemctl start
```
