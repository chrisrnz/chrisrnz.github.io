---
layout: default
title: "Ubuntu Tweaks"
published: false
date: 2017-02-06
---

# For Ubuntu 16.04.01.

## Install Base System.
(Include openssh-server task during tasksel.)

## First login.
(Add keychain launcher to .bash_profile & create a new ssh key (with password) for your user on this machine.)
```bash
echo 'if [ -f /usr/bin/keychain ]; then eval `keychain --eval --agents ssh id_dsa id_rsa`; else echo "keychain not installed" ;fi'>>.bash_profile
ssh-keygen -b 4096 -t rsa
```

### Be root.
[ref](https://finch.am/projects/sudo-on-debian/)
```bash
sudo -i
```

### Update packages & tidy.
(Start with a fresh clean slate before customization.)
```bash
apt update && apt dist-upgrade && apt autoremove && apt autoclean && apt clean
```

### Editor config.
(Unless you actually like nano.)
```bash
update-alternatives --config editor
```

### Install etckeeper to track changes.
(And ensure GIT will attribute commits appropriately.)
```bash
apt install etckeeper
git config --global --edit
```

### Enable ssh keepalives globally.
(Stops idle sessions timing out of firewall state-tables.)
```bash
echo "    ServerAliveInterval 60">>/etc/ssh/ssh_config
```

### Enable keychain for new users.
(FYI, the contents of /etc/skel/ is copied to new users home directories when user created.)
```bash
echo 'if [ -f /usr/bin/keychain ]; then eval `keychain --eval --agents ssh id_dsa id_rsa`; else echo "keychain not installed" ;fi'>>/etc/skel/.bash_profile
```

### Enable remote syslog.
(Assumes DNS name sysloghost will resolve in search-domain (check /etc/resolv.conf) fully qualify if unsure.)
```bash
echo '                *.*             @sysloghost'>/etc/rsyslog.d/51-remotesyslog.conf
service rsyslog restart
```

### Install standard tools.
(Personal preference applies; debian-goodies provides checkrestart, linux-image-extra-virtual provides some modules that linux-image-virtual doesn't (if you chose that at install time).  Season to taste. )
```bash
apt install debian-goodies linux-image-extra-virtual unzip unrar elinks inetutils-traceroute keychain snmp
```

### Install NTP.
```bash
apt install ntp
rvim /etc/ntp.conf
```

Add or comment-out appropriate time servers for your environment.
```
# Specify one or more NTP servers.
server time.yourdomain iburst
```

Test with;
```bash
service ntp restart
sleep 10
ntpq --peers
```

### Install snmpd for monitoring.
```bash
apt install snmpd
rvim /etc/snmp/snmpd.conf
```

Modify for your environment.  Ensure agentAddress is set appropriately if you wish to query remotely and if so, then consider disabling public communities.
```
#  AGENT BEHAVIOUR
#
agentAddress udp:161,udp6:[::1]:161

#  ACCESS CONTROL
#
rocommunity ****** localhost
rocommunity ****** 192.168.0.1/32
rocommunity public 192.168.0.0/24 -V systemonly
#rocommunity6 ****** 2400:ee80:2000:0500::/56 -V systemonly

#  SYSTEM INFORMATION
#
syslocation Wellington, NZ
syscontact Operations <******@******>

#  Event MIB - automatically generate alerts
#
# comment out;
#defaultMonitors          yes
#linkUpDownNotifications  yes
```

Adjust daemon options.
```bash
rvim /etc/default/snmpd
```

Tweak to make it slighly less chatty in the local logs.
```
SNMPDOPTS='-LS0-5d -Lf /dev/null -u snmp -g snmp -I -smux,mteTrigger,mteTriggerConf -p /run/snmpd.pid'
```

Restart and test.
```bash
service snmpd restart
snmpwalk -v1 -c ****** localhost | more
snmpwalk -v1 -c ****** 192.168.0.1 | more
snmpwalk -v1 -c public 192.168.0.1 | more
```

### Enable unattended updating
```bash
rvim /etc/apt/apt.conf.d/50unattended-upgrades
```

Uncomment any options you with to change from default values.
```
Unattended-Upgrade::Mail "******@*****";
Unattended-Upgrade::Remove-Unused-Dependencies "true";
Unattended-Upgrade::Automatic-Reboot "true";
```

### email config
```bash 
apt install mutt
```

The system email configuration wizard will be invoked.
```
Internet with smarthost:
accept defaults #(assumes smtp.mydomain is set correctly.)
```

Edit aliases to suit.
```bash 
rvim /etc/aliases
```

Send root's email somewhere it will get read.
```
root: yourname
```

And remember to update databases.
```bash 
postaliases
```

**More work needed here on relay ACL's if nothing else.**

### Maintenance
```bash
apt update && apt dist-upgrade && apt autoremove && apt autoclean && apt clean && checkrestart && if [ -f /var/run/reboot-required ]; then echo "REBOOT REQUIRED"; else echo "REBOOT NOT REQUIRED" ;fi
```

# Application specific amendments.

## MySQL listening on network.
(Default is listen on loopback)
### Install Server, edit config, restart and adjust users.

```bash
apt install mysql-server
rvim /etc/mysql/mysql.conf.d/mysqld.cnf
```

```
bind-address            = 0.0.0.0
```

```bash
service mysql restart
mysql -u root -p
```

```mysql
-- CREATE USER 'root'@'%' IDENTIFIED BY 'password';
-- GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
CREATE USER 'root'@'192.168.0.1' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.0.1' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

## PostgreSQL listening on network
(default is listen on loopback)

```bash
tasksel install postgresql-server
rvim /etc/postgresql/9.5/main/postgresql.conf
```

```
listen_addresses = '*'                  # (change requires restart)
```

```bash
service postgresql restart
```
**More work needed here**


## LAP Server.
**LAMP minus MySQL server, add MySQL and PostgreSQL clients instead since MySQL is running elsewhere.**
(Could use tasksel install lamp-server for this then try to prune MySQL back out afterwards.)

```bash
apt install apache2 apache2-doc apache2-suexec-custom libapache2-mod-php php-cli php-mcrypt php-mysql php-pgsql dbconfig-mysql dbconfig-pgsql
service apache2 restart
```

```markdown
**Bold** and _Italic_ and `Code` text
```
