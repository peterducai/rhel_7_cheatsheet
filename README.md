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


# SSH

```
ssh-keygen -t rsa -b 4192 -C "comment"
ssh-copy-id -i ~/.ssh/id_rsa.pub root@serverX.example.com
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
