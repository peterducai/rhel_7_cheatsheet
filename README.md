# SUDO

all commands executed using sudo are logged by default to **/var/log/secure**

# YUM

```
subscription-manager repos --disable "*" --enable rhel-7-server-optional-rpms
subscription-manager repos --enable rhel-7-server-extras-rpms
yum group install "Development Tools" --setopt=group_package_types=mandatory,default,optional
```

# Users and groups

## useradd

## userdel

When a user is removed with userdel without the -r option specified, the system will have files that are owned by an unassigned user ID number.

## usermod

```
usermod --lock/-L or --unlock/-U
usermod --append/-a --groups/-G second_group --comment/-c
usermod -s /sbin/nologin student
```

## chage

![chagedate](pic/chagedates.png)
```
chage -m 0 -M 90 -W 7 -I 14 username
chage -d 0 username will force a password update on next login.
chage -l username will list a username's current settings.
chage -E YYYY-MM-DD username will expire an account on a specific day.
```

## Groups
```
sudo groupadd -g 5000 ateam
sudo groupmod -g 6000 ateam
sudo groupdel ateam
```

# File permissions

**chmod**
* Who is u, g, o, a (for user, group, other, all)
* What is +, -, = (for add, remove, set exactly)
* Which is r, w, x (for read, write, execute)
* OR numeric is sum of r=4, w=2, and x=1.

# SSH

```
ssh-keygen -t rsa -b 4192 -C "comment"
ssh-copy-id -i ~/.ssh/id_rsa.pub root@serverX.example.com
```

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

# SELINUX

The restorecon command is part of the policycoreutil package, and semanage is part of
the policycoreutil-python package.

The **restorecon command is the preferred method** for changing the SELinux context of a file or
directory. **Unlike chcon**, the context is not explicitly specified when using this command. It uses
rules in the SELinux policy to determine what the context of the file should be.

**In most cases, running restorecon will correct the issue.**

For existing files
```
cp /tmp/file2 /var/www/html/
[root@serverX ~]# ls -Z /var/www/html/file*
-rw-r--r--. root root unconfined_u:object_r:user_tmp_t:s0 /var/www/html/file1
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/
html/file2
[root@serverX ~]# semanage fcontext -l
... 
/var/www(/.*)? all files  system_u:object_r:httpd_sys_content_t:s0  
[root@serverX ~]# semanage fcontext -l
...
/var/www(/.*)?  all files  system_u:object_r:httpd_sys_content_t:s0
...
[root@serverX ~]# restorecon -Rv /var/www/
restorecon reset /var/www/html/file1 context
```
and for new folders/files
```
semanage fcontext -a -t httpd_sys_content_t '/custom(/.*)?'
restorecon -Rv /custom
```

## Ports

```
# semanage port -l | grep -w http_port_t
http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000
```

to open port

> semanage port -a -t http_port_t -p tcp 8001

to remove port

> semanage port -d -t http_port_t -p tcp 8001

## Troubleshoot

Is logging running?

```
systemctl enable auditd.service
systemctl enable rsyslog.service
```

When SELinux denies an action, an Access Vector Cache (AVC) message is logged to the /var/log/audit/audit.log

```
tail /var/log/audit/audit.log
ausearch -m AVC,USER_AVC -ts recent
journalctl -t setroubleshoot --since=14:20
```

# Postgres

## Official postgres way

```
yum install https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-redhat11-11-2.noarch.rpm
yum install postgresql11
```

## Final steps

replace 10 with 11

```
/usr/pgsql-10/bin/postgresql-10-setup initdb
systemctl enable postgresql-10
systemctl start postgresql-10
```
