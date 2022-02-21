+++
author = "Nahid Hasan"
title = "Install and Configure SFTP"
date = "2022-02-21"
description = "How to install and configure SFTP"
tags = [
    "SSH",
    "FTP",
]
+++

Some of the developers from Nautilus project team have asked for SFTP access to at least one of the app server in Stratos DC. After going through the requirements, the system admins team has decided to configure the SFTP server on App Server 2 server in Stratos Datacenter. Please configure it as per the following instructions:



a. Create an SFTP user rose and set its password to GyQkFRVNr3.

b. Password authentication should be enabled for this user.

c. Set its ChrootDirectory to /var/www/code.

d. SFTP user should only be allowed to make SFTP connections.



Solution:

```
useradd rose
passwd rose
pass: GyQkFRVNr3

# mkdir -p /var/www/code

# ll -lsd /var/www/code
# chown root:root  /var/www

# chmod -R 755 /var/www

# ll -lsd /var/www/


# vi /etc/ssh/sshd_config

# cat /etc/ssh/sshd_config  |grep sftp -A 10

#subsystem      sftp    /usr/libexec/openssh/sftp-server

Subsystem       sftp    internal-sftp

Match User rose

ForceCommand internal-sftp

PasswordAuthentication yes

ChrootDirectory /var/www/nfsshare

PermitTunnel no

AllowTcpForwarding no

X11Forwarding no

AllowAgentForwarding no

 # Example of overriding settings on a per-user basis

#Match User anoncvs

#       X11Forwarding no



sftp rose@localhost
sftp rose@stapp02

```