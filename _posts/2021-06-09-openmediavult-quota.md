---
layout: default
categories: linux
tags: openmediavault
title: Change disk quota bigger than 4TiB on Openmediavault 5
---

# Change disk quota bigger than 4TiB on Openmediavault 5

### Disclaimer

It's possible that I forget some steps, this is just an extract what to look for.

## Terminal tools

### repquota

Show quota table

Usage: 

```shell
sudo repquota -a
```

[repquota(8): summarize quotas for filesystem - Linux man page](https://linux.die.net/man/8/repquota)

### edquota

Edit quota table

Usage, edit quota of `username` on disk `sdX`

```shell
sudo edquota -u username -f /dev/sdX
```

[edquota(8): edit user quotas - Linux man page](https://linux.die.net/man/8/edquota)

## Steps

### Change quota engine

```shell
sudo nano /etc/openmediavault/config.xml
```

Change quota engine of device to vfsv1 on the `opts` line: `jqfmt=vfsv1`. This is one of my disk as example:

```xml
...
<mntent>
        <uuid>...</uuid>
        <fsname>/dev/disk/by-uuid/...</fsname>
        <dir>/srv/dev-disk-by-uuid-...</dir>
        <type>ext4</type>
        <opts>defaults,nofail,user_xattr,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv1,acl</opts>
        <freq>0</freq>
        <passno>2</passno>
        <hidden>0</hidden>
</mntent>
...
```

Apply the change:

```shell
sudo omv-salt deploy run fstab
```

Make sure that changes are also show up in fstab:

```shell
sudo nano /etc/fstab
```

If they are not updated here, also change settings similar to the config.xml

Reboot or remount.

### Change quota with from the webui

Add some quota from the webui: Storage -> File Systems -> Quota. It's just easier to find values this way in config.xml

Than change this quota in config.xml:

```shell
sudo nano /etc/openmediavault/config.xml
```

Example values from my server:

```xml
<quota>
  <uuid>...</uuid>
  <fsuuid>...</fsuuid>
  <usrquota>
    <name>username</name>
    <bsoftlimit>0</bsoftlimit>
    <bhardlimit>9126806000</bhardlimit>
    <isoftlimit>0</isoftlimit>
    <ihardlimit>0</ihardlimit>
  </usrquota>
</quota>
```

Apply the changes:

```shell
sudo omv-salt deploy run quota
```

### Check if quota applied succesfully

```shell
sudo repquota -a
```

If config.xml and the quota table are not in sync, edit the quota table manually to the same value as in config.xml:

```shell
sudo edquota -u username -f /dev/sdX
```

```
Disk quotas for user username (uid 1000):
  Filesystem                   blocks       soft       hard     inodes     soft     hard
    /dev/sdX               8580486352          0 9126806000    3907709        0        0
```