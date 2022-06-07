# LINUX Foundation course kode kloud

```bash
# use --help to help 
# pagers are what else or what opens when it needs more

$ journalctl --help 

$ man man # will show the different man pages choices

$ apropos # will show you how to search through man pages
$ sudo mandb # will update man page 

$ apropos -s 1,8 # will filter results based on man pages sections 1 or 8
```

inode keeps track of where stuff is stored and inodes 
inode points to blocks of data required 

## hardlinks
are good to save space. points the file to the inode of target file 

inodes save data as long as hardlink is still there. well inode has a link to it. 
only hardlink to files, not folders. must be at same filesystem to create hardlink


```bash
# find command

$ find / -iname felix # make it not case sensitive
$ find -mmin 5 # modified minute 
$ find -mmin -5 # modified within last 5 minutes
$ find -mmin +5 # modified > 5 mins ago
$ find -mtime 2 # when contnets when has been changed between days. 24 hour periods. 
modification means creations or edits
modified time contents have been modified. changed time is when meta data has been changed. (like file perms)

# change time would be like meta date
$ find -ctime 3 # would look for change time like permissions within 5 mins
```
`find /lib64/ -size  +10M`

## paramater options 
c = bytes, k = kb, M = MB, G = GB

`name, iname, mmin, cmin(change minute), size, perm`

to extend expression or add more params using. 
`-name "f*" -size 512k` # AND
`-name "f*" -o -size 512k` # OR 
`-not -name "f*"` # not  can do escaped ! instead of -not 

## permissions: 
find -perm 664 # find files iwth exactly 665
find -perm -664 # find files with at least 664 permissions 
find -perm /664 # find files with any of these permissions

find -perm u=rw,g=rw,o=r
find -perm -u=rw,g=rw,o-r
find -perm /u=rw,g=rw,o=r




# Files
`chgrp` changes group for a file 
`chgrp groupName file/dir`   
  
`sudo chown owner:group`   

## SUID, SGID, and Sticky Bit 

SUID set user id bit. when file is executed will be executed as owner of the file nto the one running the file. "su command" or "passwd" command running as root 

`chmod 4664' will run file as with something. 
capital S in ls- will be ran as the user and it's noticed in by cap S
lower case `s` means it's suid bit set and it's executables 
```
    ~/test  chmod 4664 suidfile                                                        ✔ 
    ~/test  ls -l                                                                      ✔ 
total 0
-rwSrw-r-- 1 smig smig 0 May 11 12:04 suidfile
    ~/test  chmod 4764 suidfile                                                        ✔ 
    ~/test  ls -l                                                                      ✔ 
total 0
-rwSrwSr-T 1 smig smig 0 May 11 12:04 suidfile
    ~/test  chmod 0664 suidfile                                                        ✔ 
    ~/test  ls -l                                                                      ✔ 
total 0
-rw-rw-r-- 1 smig smig 0 May 11 12:04 suidfile
```

SGID same as SUID but applies to the group
SGID - takes a 2 instaed of 4 to lead it

```
~/test  chmod 2674 sguidfile                                                       ✔ 
 ~/test  ls -l                                                                      ✔ 
total 0
-rw-rwsr-- 1 smig smig 0 May 11 12:05 sguidfile
-rw-rw-r-- 1 smig smig 0 May 11 12:04 suidfile
```

find files with suid or sgid bit set... 

`find . -perm /4000 or /2000` to find suid or guid files 

`chmod 6664 fileName` set guid or suid on a file

## sticky bit 
user who owns file to only be able to delete that file 

```
    ~/test  ls -l                                                                      ✔ 
total 4
-rw-rwsr-- 1 smig smig    0 May 11 12:05 sguidfile
drwxrwxrwt 2 smig smig 4096 May 11 12:11 sticky
-rwsrw-r-- 1 smig smig    0 May 11 12:04 suidfile
```


## Cut, uniq, sort command 

```bash
$ cut -d ' ' -f 1 userinfo.txt # f= field.  d = delimeter 
$ cut -d ',' -f 3 userinfo.txt > newfilename.txt
```

a pager is something like `less` or `more`

## Boot process
systemctl list-default 
grand unified boot loader

changing default target `systemctl get-default` `systemctl set-default <target name>`


bios systems fix grub

grub2-mkconfig -o /boot/grub2/grub.cfg # will create a bios

EFI boot

grub2-mkgconfig -o /boot/efi/EFI/centos/grub.cfg 

grub2-install /dev/sda # will install group 

EFI looks for bootloader on file in special location 

`dnf reinstall grub2-efi grub2-efi-modules shim` # will fix grub config in right location 

edit grub config
`sudo /etc/default/grub` changes grub defaults or settings 

to apply changes 
`grub2-mkgconfig -o <boot dir>`

EFI uses EFI path 

### redirection 

"You use &1 to reference the value of the file descriptor 1 (stdout). So when you use 2>&1 you are basically saying “Redirect the stderr to the same place we are redirecting the stdout”. And that's why we can do something like this to redirect both stdout and stderr to the same place:

stderr -- may also output warnings not just error 


## Linux Startup processes

init = initialization system 
needs instructions to work right. the are stored in unit files 
service, socket, device, timer are example units
timer = when apps should be launched 

service units - command to use when program crashes, restarts, starts

`man systemd.service` all info on how to use service 

ExectStart # what command should run to start sshd dameon
ExecReload # what command to reoload

list unit file example 
`systemctl cat sshd.service `

edit
`sudo systemctl edit --full sshd.service`

return to default
`systemctl revert <service.name>`

if reload doesn't work try restart
`systemctl reload-or-restart` <service name> 
--now with disable will prevent them from running

when services autostart 
`sudo systemctl mask <service name>` will stop service from being auto started by other apps
`sudo systemctl unmask` 

`systemctl list-units --type service --all`

# Diagnose and Manage processes
TIME for ps is cpu time. to get time is based on % of cpu core usage 
command with brackets means kernel processes running inside the kernel 

outside of kernel is userspace is ones not running inside brackets 

`top`

`ps u 1` user oriented format of process with pid 1  
`ps -U smig` see user processes   
`ps u -U smig` user oriented format   

`pgrep -a syslog` will allow you to use grep on processes. 

niceness --- lower the number is the more cpu priority it will have

`nice -n <number nice value`   
`nice -n 11 bash`   
`ps auxl` will show nice values   
processes inherit the nice value of process that ran them   
`ps lax` all processes with long listed. 
to see inherit option  
`ps fax` will show us the tree  
`ps faux` will show the cpu and memor back with forest view   

using `losf` can list open file by process ID. 
`lsof -p 1` will show all open files by process ID 1

regular user can only assign nice values of 0 - 19. 
root is needed for negative nice levels 

run processes as regular user 
get pid then 
`sudo renice 7 <PID>`
only renice lower a session once. need sudo can raise at any time


```
[bob@centos-host ~]$ ps -ef | grep memcached
memcach+   31664       1  0 12:54 ?        00:00:00 /usr/bin/memcached -p 11211 -u memcached -m 64 -c 1024 -l 127.0.0.1,::1
bob        35625   32126  0 13:27 pts/1    00:00:00 grep --color=auto memcache
[bob@centos-host ~]$ sudo renice -n +10 31664
31664 (process ID) old priority 0, new priority 10
```

#### Process signals 
SIGSTOP / SIGKILL -- can't be ignored by any process. | last restort signal it's like end process
SIGCONT - sigcontinue 

`kill -L` | will show all signals and numerical values and signal names
`kill -SGHUP <PID>` only signals to processes owned by user if not root

`sigterm` is graceful shutdown. is default
`pkill` allows you to use process name

`pgrep -a <process name>` 


## Logging
`rsyslog` stores all logs in `/var/log/` 
can't be ready unless root
`sudo --login` new way to becoe root
`grep -r 'ssh' /var/log` way to rind a string in multiple files 

centos /usr/log/secure , /usr/log/messages, date means older logs at the end

`tail -F logname/path`

`journalctl` better than rsyslog
`journalctl /bin/sudo` will show all sudo logs 
`journalctl -u sshd.service` will show by service logs # units
`journalctl -e` # instantly go to end of the log
`journalctl -f` # follow mode for journalctl 
`journalctl -p err` will show different priotirty code names
`journalctl -p` will show all options 
`journalctl -p info -g '^b'` dash -g is grep
`journcalctl -S saturday` show since this time
`journalctl -S today -U yesterday` will show all longs from a time frame
`journcalctl -b 0` will do since current boot 
`journalctl -b -1` will show logs since previous boot
`mkdir /var/log/journal` --- will save log data 
`last`
`lastlog` last time user logged in 


# Schedule tasks to run at a set date and time 

## Cron Jobs
`cat /etc/crontab` good for syntax refresh
do not add jobs to system wide table 

cron numbers, min, hour, day of month, month, day of week 
comma can be used to match multiple values for example, 15,45 for these to intervals
- = range of values i.e. 2-4 
- / = special steps (i.e, */4) will run the job every 4 so like */4 will run every four hours 

cron jobs need full path to command 
`crontab -e` edits table as current user 
`crontab -l` see cron tab for current user 
`sudo crontab -e -u <username>`
`crontab -r` removes crontab for user 

cron tabs can be done in dirs too
/etc/cron.{daily, hourly, monthly, weekly}
make sure the file has perm read + execute so cron can run it

## anacron 
doesn't matter what time the job is ran 

`sudo vim /etc/anacrontab` syntax is documented in this file too

use 7 for period or 
@weekly / @monthly for every month 
`anacron -T` will check if file is correct format 

scheduling jobs with at
`at 15:00`

`at now + 3 hours` or `at now + 3 hours`
`atq` will show what jobs are scheduled to run 
first number is job ID
`at -c 20` will show what job contains <20> is the job number 
`atrm <job number>` will remove jobs

# Verify completion of scheduled jobs 
`/var/log/cron` is where cron jobs logs are 
on ubuntu `/var/log/syslog` by default 
CMD means logging exact commands that ran 
cron also logs commands output 
CMDOUT will show output. might need to be configured 

## anacron verify jobs 
to force anacron to run jobs
`sudo anacron -n` will run all jobs then
`sudo grep anacron /var/log/cron` filter all jobs that may have anacron 

`systemd-cat --identifier=at_schedule` will allow journalctl for output



## package managers

`dnf check-upgrade` 
`dnf repolist` 
`dnf repolist -v` will show websites for repos 
`dnf repolist --all` 
`dnf config-manager --enable powertools` will enable repo powertools 

add 3rd party repos 
`dnf config-manger --add-repo <url>` 
look at repo list
then use repo-filename to remove and then remove that file 

use single quotes on search to find two or more word phraes
`sudo dnf info <package name>`

`sudo dnf group list` to see group list
`dnf group install 'Group Name'`  
you can use --with-optional 
`dnf group remove` to remove them 

`dnf group list --hidden` 

## Identify the component of a linux distro that a file belongs to

`dnf provides /etc/anacrontab` or whatever filename 
list package or contains in package `dnf repoquery --list <package name>` 
or `dnf repoquery -l nginx` 

you can filter with grep for type of file 


# Verify integrity and availability of resources 

`tmpfs` is a file system only in memory

`du -sh /dir/location` will show how much a directory use. 's' summeraize 'h' human readable 
`free -h` see how much ram is being used 
1 Mebibyte - 

### checking file system for errors 
`sudo fsck <device>`
`sudo xfs_repair -v <device dir>`  for xfs 
`sudo fsck.ext4 -v -f -p /device dir>` f forces check, p preen mode. fix simple problems automatically, yes or no will be asked 

`systemctl list-dependencies` white light service is stopped, green is good and running. 

`chronyd` runs systems clock or time NTP time


# Kernel Runtime Params
see all kernel runtime parameters
`sysctl -a`
`sysctl -w <option>` # non persistent change 

persistent changes need to be stored in a file 

`/etc/sysctld.d/*.conf` to save the files 


`man sysctl.d` to see params

`sudo systctl -p <file path>` will make the change immediately 

# SELinux / AppArmor file 
`ls -Z` to see se linux info

`user:role:type:level `
`unconfied_u:object_r:user_home:s0`

| SELinux User | Roles |
| --- | ---|
| developer_u | developer_r, docker_r  |
| guest_u | guest_r |
| root | staff_r, sysadm_r, system_r, unconfied_r |

if role is good, checks if they can interact with the type 

type = files, domain = processes 

user -> role -> type - > level 

1. only certain users can enter certain roles and certain types 
2. allows AuthZ users and processes to do job by granting permissions
3. AuthZ can oly take limited actions
4. everything else is denied 

level is usually not actively used. (maybe in classified environments) 
level 0 - level 4. level 4 is top secret 

`ps axZ` to see processes 

only files with certain type can shift into a process or domain 

`unconfied_t` run unresctricted 
`id -Z` shows SE Linux mapping for user
`sudo semanage login -l` see where user is mapped. (unconfied_u) all access 

`getenforce` means se linux is working enforced, permissive means logging, disabled not doing anything

`sudo chcon -t`

# user accounts

`/etc/skel` dir where default user info is stored

# user management 
to create a system account user --system 
`-e ""` will make an account never expire
-r removes home directory 
`chage` allows user account expiration 

# Group Management. 

primary group - login group is the same as the main group 
`gpasswd --add <user> <group>`
`gpasswd --a <user> <group>` 
`gpasswd --d ` to remove 
`usermod -g groupname username` -- this allows to change primary group
or --gid 

`groupmod --newname oldname new name`
`groupdel` can't delete primary groups if user belongs to that group 

# Manage system-wide environment profiles

`$ printenv == $env` shows all system variables 

`/etc/environment` to change variables for every user 

if you want to run commands every time user runs in 

`/etc/profile.d/` save scripts there # knows this should be processed by bash no shebang needed 

# manage template user environment 
you can add files to `/etc/skel/`


# User Resource Limits 
`sudo vim /etc/sucurity/limits.conf` 
domain = username or @groupname, supportrs wildcards and will apply to users 
type = hard, soft, -. hard can't be overwritten by standard user, soft intial limit when user logs in, dash is hard and soft limit combined
<item> = nproc (max process,) fsize (max file size in kb),  cpu (limit in minutes)

`ulimit -a` will display current limts 
`ulimit -u 5000` will raise limit to 5000

# manage user privileges 
%wheel = user or group
ALL=(ALL) first field is host , params is run as field 
ALL = list of commands that could be executed as with sudo 
paranthesis is optional 

# configure PAM
pluggable auth modules 

pam files are stored in 
`/etc/pam.d/` 

sufficient means its okay 

`man pam.conf`


# networking

`/etc/sysconfifg/network-scripts` 
configures network devices. 

they'll use ifcfg files 

`nmtui` network manager text user interface
`dnf install nmtui` 

`sudo nmcli device reapply devicename`

## Check status of network services 

`ss`

`netstat` old version 

`ss -ltunp` 
-l listening
-t TCP connections
-u udp connections
-n -numeric values 
-p - processes

tunlp
listening,tcp,udp,numeric,process 
ways to remember 




# Packet filtering 


FirewallD - packet filtering 
puts things in zones 

Drop Zone, Trusted Zone

`firewall-cmd --get-default-zone` 
`firewall-cmd --set-default-zone=public` 

`firewall-cmd --list-all` 
`firewall-cmd --info-service=<service name>` 

`firewall-cmd --add-service=http` example adding http
`firewall-cmd --add-port=80/tcp` 

add or remove keyword interchangeable 

to create a trusted zone
`firewall-cmd --add-source=192.168.100.0/24 --zone=trusted` 

`firewall-cmd --get-acive-zones`

rules are not permanent. just add the permanent flag 

`firewall-cmd --runtime-to-permanent` will make it permanent, but not for the current section 
so add the temp then add the perm rule. or you canjust reload the firewall

# Static routing IP traffic 

`ip route add 192.168.0.0/24 via <forwarding ip>`  you can also add a dev

you can use del to delete routes 

`nmcli con show`

`nmcli c modify enp0s3 +ipv.routes "192.168.0.0/24 10.0.0.100"`
`nmcli device reapply enp0s3`
use - to remove them instead of + 

you can use nmtui to create a new route and then go to routing

# Chrony for NTP

systemctl status chronyd.service 

`sudo timedatectl set-timezone America/New_York`
timedatectl list-timezones 

dnf install chrony 
then start & enable daemon 


`sudo timedatectl set-ntp true`


# Config caching DNS server

`bind bind-utils` package 

`/etc/named.conf` 

listen-on port 53 { 127......; 192.168.0.....; }; 
any; keyword works
allow-query { localhost; };  # add ip address and cidr or use any

recursion option set to yes by default 

`named.service` is service name

`dig @localhost <host name>` 

# maintain a DNS zone 

edit named.conf 

there's a zone block 

```bash
# named.conf
zone "foo.com" IN {
    type master;
    file "foo.com.zone";
};
```
you can `/var/named/` look for `named.localhost` can be used as starting point for a zone file 

sudo cp --preserve=ownership /var/named/named.localhost /var/named/foo.com.zone 

```bash
# foo.com.zone
$TTL 1D # caching data to 1H for one hour
#SOA start of authority 
#IN means internet 
```


serial = if = 2 shoudl check for if should change 

retry = how long to wait for refresh
expire = stop trying to get requests
minimum = how long to cache negative response


# configuring email aliases 

`/var/spool/main/aaron` 

`postfix` is mail server package
`postfix.service` 

name should be @localhost 

```bash
smig@ubuntu01:~$ sudo systemctl start postfix
smig@ubuntu01:~$ sendmail smig@localhost <<< "Testing this out"
smig@ubuntu01:~$ cat /var/spool/mail/smig 
From smig@ubuntu01  Wed May 11 17:28:18 2022
Return-Path: <smig@ubuntu01>
X-Original-To: smig@localhost
Delivered-To: smig@localhost
Received: by ubuntu01 (Postfix, from userid 1000)
	id CC1E144173; Wed, 11 May 2022 17:28:18 +0000 (UTC)
Message-Id: <20220511172818.CC1E144173@ubuntu01>
Date: Wed, 11 May 2022 17:28:18 +0000 (UTC)
From: smig@ubuntu01

Testing this out
```

## create an alias 
`/etc/aliases` 
 
advertising: aaron


reload aliases 
`sudo newaliases`


multiple addresses 
`contact: usernames,username2,username3`
`advertising: aaron`
`advertising: aaron@website.com`

# configure IMAP / IMAPS service 
internet message access protocol 

imap does not encrypt
imaps / imap over ssl... TLS replaced ssl 
imaps secure 

`sudo dnf install docevot`
`dovecot.service` 
`firewall-cmd --add-service=imap` 
`firewall-cmd --add-service=imaps`

`/etc/dovecot/dovecot.conf`
#protocols = imap, pop3, whatever protocol you want
`protocols = imap` 
`listen = *, ::` dobule colon is ipv6 any and * any ipv4
can take ip addresses or whatever 

`/etc/dovecot/conf.d` dir where all the different settings are located 
10-master.conf = port locations. service imap-login (change the ports )
10-mail.conf = mail_location = mbox: 

## to secure message between mail SSL or TSL cert
`10-ssl.conf` 
ssl = required.  or yes if you want to make it optional 
ssl_{cert,key} = copy keys to the path provided, or change the path if needed 

# Restrict Access to HTTP Proxy Server

`squid` proxy package 
systemctl enable start and enable service 

firewall rule for squid 

### access rules for quid
`/etc/squid/squid.conf` 
limit domains

acl youtube dstdomain .youtube.com 

http_access deny youtube 

# Configure HTTP Server 

`httpd` `mod-ssl` packages for https 
generate tls certs
`cert bot` 

`httpd.conf` /etc/httpd/conf/httpd.conf

`ServerName` where server goes or domain name 
`DocumentRoot` /var/www/html
create new file `/etc/httpd/conf.d/two-websites.conf`

needs virtual host
ServerName 
DocumentRoot
</virtual host>

each site needs virtaul host and servername, doc root
you can also do virtual hosts with a private ip for intranet and then have a public ip for public facing IPs

`apachectl configtest` will verify config is good 

SSLEngine on 
SSLCertificateFile "/path/to/file.cert"
SSLCertificateKeyFile "/path/To/KeyFile
ssl conf files 

edit html conf files modules 
`/etc/httpd/conf.modules.d/` where http modules live

# HTTP server log files
error log and access log 
default logs are stored in main conf file 
ErrorLog "location/where/logs are stored" saves at server root by default 
can also use an absolute path 

LogLevel warn

add to virtualhosts
CustomLog create a path
ErrorLog create a path

to have log formats for those sites 

# disabling sites
add .disabled to sites conf file extension 

AllowOverride None # way to config special htaccess files
AllowOverride All # will allow htaccess files 

`Require all granted `

create directory block 
Require all denied 
Require ip 192.168.100 192.168.1.79  <can use cidr as well>

Files ".ht*" directive 

`htpasswd -c /etc/httpd/passwords ausername`
-c creates file or updates it 
-D deletes password 

Directory 
AuthType Basic
AuthBasicProvider file
AuthName 
AuthUserFile /path/to/file
Require valid-user 

# Config Db server
`mariadb-server` 
`firewall-cmd --add-service=mysql`

default config for mariadb 
`mysql -u root` full perm to database, etc

## harden mysql
`mysql_secure_installation`  to harden it
set a root password 
remove anonymous users
disallow root login # prevent apps on other remote clients to access root user
remove test db and access
reload priv tables 

`mysql -u root -p` 

## config files
`/etc/my.cnf` 
`/etc/my.cnf.d/mariadb-server.cnf` 
socket directive = allows apps to connect to maria db at default location 

`bind-address`  may want to limit access by adding bind addy 

# manage and config containers 

podman `docker` uses same commands as docker

`/etc/containers/registries.conf`

comment unqualified-search-registires 
set it to ['docker.io'] which makes default repo to docker 

create `/etc/containers/nodocker` 


# Configure FIle Systems

format xfs fs 
`sudo mkfs.xfs /dev/sdX`
`mkfs.xfs` will show you options too 
larger inode size means more metadata 
`-i size=512` changes inode size 

`xfs` and tab will show xfs utilities 
`sudo xfs_admin -l ` will display label 

if you run out of inodes the disk will appear full. so you need more inodes space 

-N "500000" is a good number for ext4 
`tunee2fs` to modify ext4 fs

# Mount FS at or during boot

`systemctl daemon-reload` updates from fstab or other changes if you made changes. 


# Configure Systems to mount fs on demand

##On Demand Mounting 
`autofs` 
start and enable service 

`nfs-utils` package start nfs-server daemon
`/etc/exports` 
```
/etc  127.0.0.1(ro)
```

`/etc/auto.master` will unmount after 300 seconds 
--timeout=400 # will make it 400

`/shares/ /etc/auto.shares --timeout=400` if dir does not exist it will be created  

create file 
```
mynextworkshare -fstype=auto (mount options) server address
```

reload autofs after creating stuff

```
#auto.master
/- /share/location # to get rid of dir

#auto.shares file
/absolutepath 
```

# Evaluate / compare basic file system features and options

`findmnt` shows all mount points 
`findmnt -t xfs` will show only xfs filesystems

## mount options 
give file system behaviors 
noexec - can't launch a program on this filesysem. no execute 
(androids utilize this)
nosuid - (to prevent suid stuff)
`sudo mount -o ro,noexec,nosuid`
you can use remount option to update mount options


# Manage and Configure LVM Storage 

`lvm2` package for installing LVM
PE = physical extent 

`# lvmdiskscan` will show you partitions and what's used for lvm
`# pvs` 

`sudo lvresize --resizefs --size 3G volume/partion` 

# Create and config encrypted storage 

`# cryptsetup --verify-passphrase open --type plain /dev/loc mysecuredisk`
open action means read and write encrypted data
--type plain - uses encryption scheme called plain 
mysecuredisk is name for mapped device 

`sudo cryptsetup close mysecuredisk`

## Luks encryption
`sudo cryptsetup luksFormat /dev/vde`

`sudo cryptsetup luksChangeKey /dev/vde`
`sudo cryptsetup open /dev/vde mysecuredisk`
uses header and it detects the encryption type 

## to only encrypt part of the disk
specify the partition 

# Create and Mange RAID

Redudant Array of Independent Disks
Level 0 RAID striped array - linux views them as single storage area - no redundancy 

Level 1 - mirrored disk 

Level 5 - min 3 disks. If one disk fails it's okay. disk parity ... .33 saved

Level 6 - lose two disks and it's okay. need 4 disks min

Level 10 or Raid + 1 and 0 - 

## create RAIDS multiple device admin
`sudo mdadm --create /dev/md0 --level=0 --raid-devices=3 /dev/...` 
md0 is block device for raids 

`sudo mdadm --stop /dev/md0`

linux scans for superblock of all devices. Linux will automatically will create it into an erray

`sudo mdadm --zero-superblock <devices>` will prevent that

how to add spare disk to array

`# mdadm --create /dev/md0 --level=1 --raid-devices=2 <devices> --spare-device=1 <device>` 

`# mdadm --magage /dev/md0 --add <dev>`

`cat /proc/mdstat` see raid types or personalities 

## Advanced fs permissions 

`setfacl` 
`sudo setfacl --modify user:aaron:rw <filename>`

`getfacl <filename>` to see acl for file 

mask in facls ... max permissions file or dir can have 
`sudo setfacl --modify mask:r <filename>`
makes #effective field means user can only do what makes says

`sudo setfacl --modify group:wheel:rw <filename>` will allow root to do that
`sudo setfacl --modify user:aaron:--- <filename>` will remove users permissions
`sudo setfacl --remove acl:choices: <filename>` 

facls recursively 
`sudo setfacl --recursive  -m user:aaron:rwx`

### File and directory attributes 
only root can change append only attribute (enable or disable) 
`sudo chattr +a filePath` that means you can only append to a file, can't change what was there before 

`sudo chattr +i filePath` will add immutable 
can't be changed in anyway 

`lsattr` to find attributes


# quotas
`quota`

add fs options  in fstab`usrquota,grpguota` to add quotas to file systems
xfs tracks quotas internally in the file system

ext4 does not support quotas of break. so need to run this
`sudo quotacheck --create-files --user --group /device/`
`sudo quotaon /mountpoint`

`fallocate --length 100M`

`sudo edquota --user aaron` 
1 block = 1KB. 
Soft and Hard limits. 0 = no limits

`sudo quota --edit-period` 
you can change the grace periods here


`sudo xfs_quota -x -c 'limit bsoft=100m bhard=500m john' /mnt/`


## Resource Limits 

Edit /etc/security/limits.conf file:


sudo vi /etc/security/limits.conf


And add below given line at the end of the file:

@jane hard nproc 30


