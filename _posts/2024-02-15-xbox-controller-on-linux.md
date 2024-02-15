---
layout: default
categories: linux
tags: gaming
title: Xbox controller on Linux
---

# Xbox controller on Linux with xpadneo with onboard Intel AX210

Most basic steps are detailed in the ArchWiki, here are just some more details, which helped me.

ArchWiki section: [Gamepad - ArchWiki](https://wiki.archlinux.org/title/Gamepad#xpadneo)

My bluetooth adapter uses the same IOMMU groups as a lot of other devices, so VFIO wasn't usable.

### 1. Install [xpadneo-dkms](https://github.com/atar-axis/xpadneo)

E.g. on Arch with yay:

```shell
yay xpadneo-dkms
```

### 2. Install and setup win10/11 VM in QEMU/KVM

Some changes required for USB passthrough to work. [Source reddit comment](https://www.reddit.com/r/VFIO/comments/sdctt2/bluetooth_device_passthrough_intel_bluetooth/j917991/) [Other Reddit thread](https://www.reddit.com/r/VFIO/comments/wbsqy1/how_to_fix_onboard_intel_bluetooth_error_code_10/)

In the xml update the schema and add capability to the end:

```xml
<domain xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0" type="kvm">  
  <devices>    
...    
  </devices>    
  <qemu:capabilities>    
<qemu:del capability="usb-host.hostdevice"/>    
  </qemu:capabilities>    
</domain>
```

Then Add hardware -> USB Host device -> The bluetooth adapter

### 3. Pair devices in Windows

Push the connect button, pair controller the normal way

Shut down the VM

### 4. Pair devices in Linux

Push the connect button, pair controller the normal way



Now it works!


