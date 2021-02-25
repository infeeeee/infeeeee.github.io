---
layout: default
categories: linux
tags: openmediavault
title: Openmediavault 5 install notes
---

# Openmediavault 5 install notes

Steps I had to do manually again during install.

## Ssh

On ssh pubkey auth install if I get the error in log:

```
bad ownership or modes for directory
```

- Login with password on ssh

- `sudo salt-call --local state.orchestrate omv.setup.fixmode`

Source: [ssh with public key - bad ownership or modes for directory - Seite 2 - SSH - openmediavault](https://forum.openmediavault.org/index.php?thread/23234-ssh-with-public-key-bad-ownership-or-modes-for-directory/&postID=250900#post250900)

## Smart

Hdd safe temps:

Informal: 45

Critical:55

## omv-extras

Documentation is here:

[OMV-Extras.org Plugin - Guides - openmediavault](https://forum.openmediavault.org/index.php?thread/5549-omv-extras-org-plugin/)

Download plugin as deb from here:

https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/openmediavault-omvextrasorg_latest_all5.deb

## QEMU-KVM

### Virt-manager from remote

To access libvirt from a remote machine from virtmanager

- Add the user to the `libvirt` group

- Install nc, this version works only!

```shell
sudo apt install netcat-openbsd  
```

- On the remote machine ssh from terminal before virt manager!

- It should work now

### Bridged network

*This part is a work in progress, I still didn't find a best solution.*

- ~~Install bridge-utils:~~
  
  *I'm not 100% sure if this is needed*
  
  ```shell
  sudo apt install bridge-utils
  ```

OMV5 uses [netplan.io](https://netplan.io/). To create a bridge with netplan:

```shell
cd /etc/netplan
sudo nano 30-bridge.yaml
```

Obviously, use a bigger number than 30 if there are more files in this directory. The contents of the file:

```yaml
network:
  bridges:
    br0:
      interfaces: [enp0s25]
      dhcp4: true
      dhcp6: no
```

For the interface use the name of the ethernet interface.

If this folder is empty you can regenerate networks from `omv-firstaid`>`configure network interfaces`

Reboot the server (it should be enough to reload networking, but it always messes up ip addresses, so it's just easier to simply reboot)

Bridged network should work now

## Docker

### Docker and bridge network

Docker breaks libvirt bridge network:

[iptables - Docker breaks libvirt bridge network - Server Fault](https://serverfault.com/questions/963759/docker-breaks-libvirt-bridge-network)

My solution is from the question itself, as I never expose vms to the outside world, so they are already behind the router's firewall. It's also important to install `iptables-persistent` and `netfilter-persistent`. As `iptables-persistent` saves the current rules during install, it's recommended to set up the rule first, than install `iptables.persistent`

```shell
sudo iptables -I FORWARD -i br0 -o br0 -j ACCEPT
sudo apt install iptables-persistent netfilter-persistent
```

Saving the iptables after installation: (Piping doesn't work with sudo)

```shell
sudo su
iptables-save > /etc/iptables/rules.v4
exit
```