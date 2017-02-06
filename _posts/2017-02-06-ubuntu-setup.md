---
layout: default
title: "Ubuntu Tweaks"
published: false
date: 2017-02-06
---

# Ubuntu 16.04.01

## Install Base System plus openssh-server task.

## First login
```bash
echo 'if [ -f /usr/bin/keychain ]; then eval `keychain --eval --agents ssh id_dsa id_rsa`; else echo "keychain not installed" ;fi'>>.bash_profile
ssh-keygen -b 4096 -t rsa
```

### Get root
```bash
sudo -i
```

### Update packages & tidy.

```bash
apt update && apt dist-upgrade && apt autoremove && apt autoclean && apt clean
```

### editor config
```bash
update-alternatives --config editor
```

### Install etckeeper to track changes

```bash
apt install etckeeper
git config --global --edit
```

### Enable ssh keepalives globally
```bash
echo "    ServerAliveInterval 60">>/etc/ssh/ssh_config
```

### Enable keychain for new users.
```bash
echo 'if [ -f /usr/bin/keychain ]; then eval `keychain --eval --agents ssh id_dsa id_rsa`; else echo "keychain not installed" ;fi'>>/etc/skel/.bash_profile
```

### Enable remote syslog.
```bash
echo '                *.*             @syslog'>/etc/rsyslog.d/51-remotesyslog.conf
service rsyslog restart
```

### Install standard tools
```bash
apt install debian-goodies linux-image-extra-virtual unzip unrar elinks inetutils-traceroute keychain snmp
```

### Install ntp
```bash
apt install ntp
rvim /etc/ntp.conf && service ntp restart
```

`# Specify one or more NTP servers.
server time.hardknocks.school.nz iburst`

```bash
ntpq --peers
```

### Install snmpd for monitoring.
```bash
apt install snmpd
rvim /etc/snmp/snmpd.conf
```

`agentAddress udp:161,udp6:[::1]:161
rocommunity ****** localhost
rocommunity ****** 192.168.0.1/32
rocommunity public 192.168.0.0/24 -V systemonly
#rocommunity6 ****** 2400:ee80:2000:0500::/56 -V systemonly
syslocation Wellington, NZ
syscontact Operations <******@******>

# comment out;
line 151: Warning: Unknown token: defaultMonitors.
line 153: Warning: Unknown token: linkUpDownNotifications.`

```bash
rvim /etc/default/snmpd
```

`SNMPDOPTS='-LS0-5d -Lf /dev/null -u snmp -g snmp -I -smux,mteTrigger,mteTriggerConf -p /run/snmpd.pid'`

```bash
service snmpd restart
snmpwalk -v1 -c ****** localhost
snmpwalk -v1 -c ****** 192.168.0.1
snmpwalk -v1 -c public 192.168.0.1
```

### Enable unattended updating
```bash
rvim /etc/apt/apt.conf.d/50unattended-upgrades
```
`Unattended-Upgrade::Mail "******@*****";
Unattended-Upgrade::Remove-Unused-Dependencies "true";
Unattended-Upgrade::Automatic-Reboot "true";`

### email config
```bash 
apt install mutt
```

`Internet with smarthost:
accept defaults #(assumes smtp.mydomain is set correctly.)`

```bash 
rvim /etc/aliases
```

`root: yourname`

```bash 
postaliases
```

### Maintenance
```bash
apt update && apt dist-upgrade && apt autoremove && apt autoclean && apt clean && checkrestart && if [ -f /var/run/reboot-required ]; then echo "REBOOT REQUIRED"; else echo "REBOOT NOT REQUIRED" ;fi
```

```markdown
**Bold** and _Italic_ and `Code` text
```
