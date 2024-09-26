# DNS server setup for Centos9
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
## Setting up named.conf file

```bash
[root@dn1 ~]# vim /etc/named.conf
```
Find and modifie for your needs.
```bash
options {
        listen-on port 53 { 127.0.0.1; 192.168.1.210; }; # Type in the address of this machine
        listen-on-v6 port 53 { none; }; # You can also add ipv6 address or if you don't want ipv6 dns you can input none
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; 192.168.1.0/24; }; # Input your subnet/subnets from where dnf queries will be allowed.
```
Scroll down until you see "zone"
```bash
zone "lokalus.netas" IN {  # Type in your domain name.
        type master;
        file "lokalus.netas.db"; # Enter your zone filename
        allow-update { none; };
};
```
Modify your zone file.
```bash
[root@dn1 ~]# cp /var/named/named.empty /var/lokalus.netas.db # Here you can copy the template file or create a new one from scratch.
```
```bash
[root@dn1 ~]# vim /var/named/lokalus.netas.db # Open your zone file
```
```bash
$TTL 1D
@       IN SOA  dn1.lokalus.netas.  (  # Here you will need to add your dns servers hostname and if you have the mail server you will need to add it also.
                                        0       ; serial
                                        1D      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum

        IN      NS      dn1.lokalus.netas. # Type in your dns server hostname

dn1     IN      A        192.168.1.210   # This is the host records add as much as you need.
main    IN      A        192.168.1.178
```

