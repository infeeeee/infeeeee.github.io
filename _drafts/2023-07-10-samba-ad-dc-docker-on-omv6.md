---
layout: default
categories: linux
tags: openmediavault
title: Samba AD-DC in Docker on OpenMediaVault6
---

# Samba AD-DC in Docker on OpenMediaVault6

OMV6 + ZFS + AD + Fileserver

Based on this guide: https://github.com/Fmstrat/samba-domain


## Search and replace

Case sensitive!

| IP addresses          | Example value   |
| --------------------- | --------------- |
| Host IP               | `192.168.3.221` |
| DC IP                 | `192.168.3.222` |
| Main DNS, e.g. router | `192.168.3.1`   |

| Domain names | Example value               |
| ------------ | --------------------------- |
| DOMAIN       | `CORP.EXAMPLE.COM`          |
| dns search   | `corp.example.com`          |
| DOMAIN_DC    | `dc=corp,dc=example,dc=com` |
| DOMAIN_EMAIL | `example.com`               |
| DOMAINPASS   | `ThisIsMyAdminPassword^123` |
| dc           | `exampledc`                 |
| Fileserver   | `MYSERVERHOSTNAME`          |


| Users and groups | Example value        |
| ---------------- | -------------------- |
| Admin user       | `CORP\administrator` |
| Local group 1    | `@"CORP\group1 lg"`  |
| Local group 2    | `@"CORP\group2 lg"`  |

| Filesystems and shares | Example value |
| ---------------------- | ------------- |
| Zfs pool               | `zpool`       |
| Filesystem/share 1     | `share1`      |
| Filesystem/share 2     | `share2`      |

## Notes before start

- Make sure time and timezone is set up correctly
- Select a suitable domain name


## Netplan config

Get 2 addresses without DHCP in Netplan. Edit after omv generated this file.


`/etc/netplan/20-openmediavault-enp1s0.yaml`

```yaml
network:
  ethernets:
    enp1s0:
      match:
        macaddress: 52:54:xx:xx:xx:xx
      addresses:
        - 192.168.3.221/24
        - 192.168.3.222/24
      routes:
       - to: 0.0.0.0/0
         via: 192.168.3.1
      nameservers:
        addresses:
          - 192.168.3.1
          - 1.1.1.1
      dhcp4: no
      dhcp6: no
      link-local: []
```

## Compose

Omv-extras `Compose` addon works as expected. This compose started without any problems:

https://github.com/Fmstrat/samba-domain#examples-with-docker-compose


```yaml
version: '2'
services:

  samba:
    image: nowsci/samba-domain
    container_name: samba
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /docker/data/samba:/var/lib/samba
      - /docker/config/samba:/etc/samba/external
      # zfs filesystems, more about this later:
      - /zpool/share1:/storage/share1:rw
      - /zpool/share2:/storage/share2:rw
    environment:
      - DOMAIN=CORP.EXAMPLE.COM
      - DOMAIN_DC=dc=corp,dc=example,dc=com
      - DOMAIN_EMAIL=example.com
      # Delete this after first run:
      - DOMAINPASS=ThisIsMyAdminPassword^123
      - DNSFORWARDER=192.168.3.1
      - HOSTIP=192.168.3.222
    ports:
      - 192.168.3.222:53:53
      - 192.168.3.222:53:53/udp
      - 192.168.3.222:88:88
      - 192.168.3.222:88:88/udp
      - 192.168.3.222:123:123
      - 192.168.3.222:123:123/udp
      - 192.168.3.222:135:135
      - 192.168.3.222:137-138:137-138/udp
      - 192.168.3.222:139:139
      - 192.168.3.222:389:389
      - 192.168.3.222:389:389/udp
      - 192.168.3.222:445:445
      - 192.168.3.222:464:464
      - 192.168.3.222:464:464/udp
      - 192.168.3.222:636:636
      - 192.168.3.222:1024-1044:1024-1044
      - 192.168.3.222:3268-3269:3268-3269
    dns_search:
      - corp.example.com
    dns:
      - 192.168.3.222
      - 192.168.3.1
    extra_hosts:
      - exampledc.corp.example.com:192.168.3.222
    hostname: exampledc
    cap_add:
      - NET_ADMIN
      - SYS_NICE
      - SYS_TIME
    devices:
      - /dev/net/tun
    privileged: true
    restart: unless-stopped

```

## Setup on Windows

### Join from Win11

#### Network setup

Settings ➡ Network & internet ➡ Ethernet

   1. Network profile type: Private
   2. DNS server assignment: manual and `192.168.3.222`

If client got an IPv6 address:

   1. Control Panel ➡ Network and Internet ➡ Network Connections
   2. Ethernet ➡ Properties ➡ TCP/IPv6 ➡ Untick

#### Join the domain

1. Settings ➡ Accounts ➡ Access work or school ➡ Add a work or school account ➡ Connect
2. Join this device to a local Active Directory domain
3. Domain name: `CORP.EXAMPLE.COM`
4. User: `administrator` Password: `ThisIsMyAdminPassword^123`

### Edit domain settings

RSAT: https://wiki.samba.org/index.php/Installing_RSAT

Built into windows, as optional features

### Groups

Local Groups:
- Members: Global groups, resources
- Assign permissions, Group policies

Global Groups:
- Members: Users

## ZFS

Create separate filesystem for each shared folder! Otherwise file hitory won't work from Windows.

### Auto snapshot

This just works™: https://github.com/zfsonlinux/zfs-auto-snapshot

Installs and sets up everything, cronjobs.

### Monthly Auto Scrub, OMV WebUI

To run scrub every monthly, always the first friday, at 20:10

| Option            | Value                                             |
| ----------------- | ------------------------------------------------- |
| Time of execution | Certain date                                      |
| Minute            | 10                                                |
| Hour              | 20                                                |
| Day of month      | *                                                 |
| Month             | *                                                 |
| Day of week       | Friday                                            |
| User              | root                                              |
| Command           | `[ $(date +\%d) -le 07 ] && zpool scrub zfs-pool` |


The first part of the command checks the number of the day, yo it only runs on the first week


## File sharing

Again, mostly based on the docker docs

smb.conf:

```
# Global parameters
[global]
    dns forwarder = 192.168.3.1
    idmap_ldb:use rfc2307 = yes
            wins support = yes
            template shell = /bin/bash
            template homedir = /home/%U
            idmap config EXAMPLE : schema_mode = rfc2307
            idmap config EXAMPLE : unix_nss_info = yes
            idmap config EXAMPLE : backend = ad
    # dns forwarder = 127.0.0.11 comment this line! bug!
    netbios name = EXAMPLEDC
    realm = CORP.EXAMPLE.COM
    server role = active directory domain controller
    workgroup = EXAMPLE
    idmap_ldb:use rfc2307 = yes


    # File sharing:
    security = user
    passdb backend = ldapsam:ldap://localhost
    ldap suffix = dc=corp,dc=example,dc=com
    ldap user suffix = ou=Users
    ldap group suffix = ou=Groups
    ldap machine suffix = ou=Computers
    ldap idmap suffix = ou=Idmap
    ldap admin dn = cn=Administrator,cn=Users,dc=corp,dc=example,dc=com
    ldap ssl = off
    ldap passwd sync = no
    server string = MYSERVERHOSTNAME
    wins support = yes
    preserve case = yes
    short preserve case = yes
    default case = lower
    case sensitive = auto
    preferred master = yes
    unix extensions = yes
    follow symlinks = yes
    client ntlmv2 auth = yes
    client lanman auth = yes
    mangled names = no

    # zfs snapshots:
    vfs objects = dfs_samba4 acl_xattr shadow_copy2
    shadow: snapdir = .zfs/snapshot
    shadow: sort = desc
    shadow: format = -%Y-%m-%d-%H%M
    shadow: snapprefix = ^zfs-auto-snap_\(frequent\)\{0,1\}\(hourly\)\{0,1\}\(daily\)\{0,1\}\(monthly\)\{0,1\}
    shadow: delimiter = -20

    # custom:
    inherit permissions = yes

[sysvol]
    path = /var/lib/samba/sysvol
    read only = No

[netlogon]
    path = /var/lib/samba/sysvol/corp.example.com/scripts
    read only = No

[share1]
    comment = share1
    path = /storage/share1
    public = no
    read only = no
    writable = yes
    write list = CORP\administrator, @"CORP\group1 lg", @"CORP\group2 lg"
    guest ok = no
    valid users = CORP\administrator, @"CORP\group1 lg", @"CORP\group2 lg"

[share2]
    comment = share2
    path = /storage/share2
    public = no
    read only = no
    writable = yes
    write list = CORP\administrator, @"CORP\group1 lg"
    guest ok = no
    valid users = CORP\administrator, @"CORP\group1 lg"
```