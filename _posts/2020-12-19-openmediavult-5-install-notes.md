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

Hdd safew temps:

Informal: 45

Critical:55

## omv-extras

Documentation is here:

[OMV-Extras.org Plugin - Guides - openmediavault](https://forum.openmediavault.org/index.php?thread/5549-omv-extras-org-plugin/)

Download plugin as deb from here:

https://github.com/OpenMediaVault-Plugin-Developers/packages/raw/master/openmediavault-omvextrasorg_latest_all5.deb

### QEMU-KVM

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

- Install bridge-utils: 
  
  ```shell
  sudo apt install bridge-utils
  ```

- Find device name, with `ip a` something like `enp0s25`

- ```shell
  sudo virsh iface-bridge enp0s25 br0
  ```

- Check if bridge is in `/etc/network/interfaces`

- Reboot the server (it should be enough to reload networking, but it always messes up ip addresses, so it's just easier to simply reboot)

- Bridged network should work now