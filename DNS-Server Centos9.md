# Setting up system
First we need to update the system..

```bash
[root@localhost ~]# dnf update && dnf upgrade
```
Install required packages..

```bash
[root@localhost ~]# dnf -y install bind bind-utils 
```
Set system hostname to your needs, --pretty is the short version of hostname.

```bash
[root@dn1 ~]# hostnamectl hostname dn1.lokalus.netas
[root@dn1 ~]# hostnamectl hostname dn1 --pretty
```
Add your hostname and the IP address of the machine to hosts file.
```bash
[root@dn1 ~]# vim /etc/hosts
192.168.1.210 dn1.lokalus.netas dn1
```
Try to ping your local machine with the hostname.
```bash
[root@dn1 ~]# ping -c3 dn1
[root@dn1 ~]# ping -c3 dn1.lokalus.netas
```

