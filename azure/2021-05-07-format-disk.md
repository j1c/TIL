---
layout: post
title: Making disk volumes available in Azure VMs
tags: [Azure, TIL]
author: Jaewon Chung
comments : True
---

This article shows you how to attach both new and existing disks to a Linux virtual machine through the Azure portal.

1. Connect to your Azure virtual machine instance.
2. Use the `lsblk` command to view your available disk devices and their mount points (if applicable) to help you determine the correct device name to use. The output of lsblk removes the /dev/ prefix from full device paths.

```{bash}
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
```

should output something similar to
```{bash}
sda     0:0:0:0      30G
├─sda1             29.9G /
├─sda14               4M
└─sda15             106M /boot/efi
sdb     1:0:1:0      14G
└─sdb1               14G /mnt
sdc     3:0:0:0       512G
```
The root device is `sda`, and attached volume is `/dev/sdc`, which is not yet mounted.

3. We can verify if any file system exists in the volume.
```
sudo file -s /dev/sdc
```
If the output shows simply `data`, as in the following example output, there is no file system on the device
```
/dev/sdc: data
```
4. If you are using an existing disk that contains data, skip to mounting the disk. If you are attaching a new disk, you need to partition the disk.

The `parted` utility can be used to partition and to format a data disk.

The following example uses `parted` on `/dev/sdc`, which is where the first data disk will typically be on most VMs. Replace `sdc` with the correct option for your disk. We are also formatting it using the XFS filesystem.

```
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfs.xfs /dev/sdc1
sudo partprobe /dev/sdc1
```

5. Use the `mkdir` command to create a mount point directory for the volume. The mount point is where the volume is located in the file system tree and where you read and write files to after you mount the volume. The following example creates a directory named `/data`.
```
sudo mkdir /data
```

6. Use the following command to mount the partitioned volume at the directory you created in the previous step.

```
sudo mount /dev/sdc1 /data
```

7. The volume will not automatically attach to `/data` when the machine reboots. To mount an attached volume on every system reboot, add an entry for the device to the /etc/fstab file. Run
```
sudo blkid
```
should output something similar to:
```
/dev/sda1: LABEL="cloudimg-rootfs" UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4" PARTUUID="1a1b1c1d-11aa-1234-1a1a1a1a1a1a"
/dev/sda15: LABEL="UEFI" UUID="BCD7-96A6" TYPE="vfat" PARTUUID="1e1g1cg1h-11aa-1234-1u1u1a1a1u1u"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4" TYPE="ext4" PARTUUID="1a2b3c4d-01"
/dev/sda14: PARTUUID="2e2g2cg2h-11aa-1234-1u1u1a1a1u1u"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="xfs" PARTLABEL="xfspart" PARTUUID="c1c2c3c4-1234-cdef-asdf3456ghjk"
```

Next, open the /etc/fstab file in a text editor as follows:
```
sudo nano /etc/fstab
```
Add the following entry to `/etc/fstab` to mount the device at the specified mount point. The fields are the UUID value returned by `blkid`, the mount point, the file system, and the recommended file system mount options.

In the following example, we mount the device with UUID `33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e` to mount point `/data` and we use the `xfs` file system. We also use the defaults and nofail flags. We specify `0` to prevent the file system from being dumped, and we specify `2` to indicate that it is a non-root device.

```
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e  /data  xfs  defaults,nofail  0  2
```

Sources:
- [Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal
)
- [AWS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)