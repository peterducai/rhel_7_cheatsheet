# Understand and use essential tools

Access a shell prompt and issue commands with correct syntax
Use input-output redirection (>, >>, |, 2>, etc.)

> command 2>&1 1>file 

does not do the same as 

> command 1>file 2>&1

Use grep and regular expressions to analyze text
Access remote systems using ssh
Log in and switch users in multiuser targets
Archive, compress, unpack, and uncompress files using tar, star, gzip, and bzip2

```
g(un)zip file
b(un)zip2 file
```

To archive and compress a directory (with the SELinux contexts), type:

> tar --selinux -czvf directory.tgz directory

```
# tar (c/x/t)zvf directory.tgz (--selinux)
# tar (c/x/t)jvf directory.bz2 (--selinux)
```

Create and edit text files
Create, delete, copy, and move files and directories
Create hard and soft links
List, set, and change standard ugo/rwx permissions
Locate, read, and use system documentation including man, info, and files in /usr/share/doc

```
yum install -y selinux-policy-devel
mandb
man ps
whatis ps
apropos ps   # search for *ps*
mandb        # update man
```

Note: Red Hat may use applications during the exam that are not included in Red Hat Enterprise Linux for the purpose of evaluating candidate's abilities to meet this objective.

# Operate running systems

Boot, reboot, and shut down a system normally
Boot systems into different targets manually

Interrupt the boot process in order to gain access to a system

```
press e to edit grub config
on kernel line put.. rd.break enforcing=0
ctr+x


Identify CPU/memory intensive processes, adjust process priority with renice, and kill processes
Locate and interpret system log files and journals

```
sudo systemd-analyze  blame
sudo journalctl -p err
```

permanent logging

```
mkdir /var/log/journal   (instead of /run/log/journal)
echo "SystemMaxUse=50M" >> /etc/systemd/journald.conf
systemctl restart systemd-journald 
```

Access a virtual machine's console
Start and stop virtual machines
Start, stop, and check the status of network services
Securely transfer files between systems 

# Configure local storage

List, create, delete partitions on MBR and GPT disks
Create and remove physical volumes, assign physical volumes to volume groups, and create and delete logical volumes
Configure systems to mount file systems at boot by Universally Unique ID (UUID) or label
Add new partitions and logical volumes, and swap to a system non-destructively

# Create and configure file systems

Create, mount, unmount, and use vfat, ext4, and xfs file systems
Mount and unmount CIFS and NFS network file systems

```
yum install -y nfs-utils
"nfsserver:/home/tools /mnt nfs4 defaults 0 0" to /etc/fstab
mount -a && mount|grep nfserver
umount /mnt
fuser /mnt (if device busy)
```

```
yum install -y cifs-utils
yum install -y samba-client
"//smbserver/shared /mnt cifs rw,username=user01,password=pass 0 0" to /etc/fstab
OR
credentials=/root/.credentials  where creds will be stored (and chmod 0600)
mount -a
```

Extend existing logical volumes

```
pvs
vgs
lvs
```

Create and configure set-GID directories for collaboration

```
groupadd -g 50000 team
mkdir /home/shared
chown nobody:team /home/shared
chmod g+ws,o-rwx /home/shared
chmod 2770 /home/shared
useradd user0X; usermod -aG team user0X
```

Finally, if you want the team group members to be able to see each other’s files but not to delete them, type:

```
# chmod +t /home/shared
```

> find all SUID: find / -perm +4000

Create and manage Access Control Lists (ACLs)
Diagnose and correct file permission problems

# Deploy, configure, and maintain systems

Configure networking and hostname resolution statically or dynamically
Schedule tasks using at and cron
Start and stop services and configure services to start automatically at boot
Configure systems to boot into a specific target automatically
Install Red Hat Enterprise Linux systems as virtual guests
Configure systems to launch virtual machines at boot
Configure network services to start automatically at boot

> nmcli con mod static2 connection.autoconnect no

<!-- nmcli con mod static2 connection.permissions user:stella,john -->

Configure a system to use time services
Install and update software packages from Red Hat Network, a remote repository, or from the local file system
Update the kernel package appropriately to ensure a bootable system
Modify the system bootloader

> rd.break enforcing=0

# Manage users and groups

Create, delete, and modify local user accounts
Change passwords and adjust password aging for local user accounts
Create, delete, and modify local groups and group memberships
Configure a system to use an existing authentication service for user and group information

# Manage security

Configure firewall settings using firewall-config, firewall-cmd, or iptables
Configure key-based authentication for SSH
Set enforcing and permissive modes for SELinux
List and identify SELinux file and process context
Restore default file contexts
Use boolean settings to modify system SELinux settings
Diagnose and address routine SELinux policy violations
