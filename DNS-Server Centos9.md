# Setting up system
First we need to update the system..

'''
[root@dn1 ~]# dnf update && dnf upgrade
'''
Install required packages..

'''bash
[root@dn1 ~]# dnf -y install bind bind-utils 
'''
Set system hostname to your needs, --pretty is the short version of hostname.

'''bash
[root@dn1 ~]# hostnamectl hostname dn1.lokalus.netas
[root@dn1 ~]# hostnamectl hostname dn1 --pretty
'''

'''bash
[root@dn1 ~]# vim /etc/hosts
'''
