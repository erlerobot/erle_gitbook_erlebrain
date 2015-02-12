# `chroot` to Erle-Brain's image

Sometimes, for development purposes (packaging, ROS packages test, filesystem re-design, etc) we want to edit Erle-Brain's image from our Linux external box. This tutorial will explain how to do so and run commands in your Linux box as if you were in front of Erle-Brain's command line.

### Inspecting the image

We need to know hot the image is partitioned before mounting it in our local machine. To do so we execute:
```bash
fdisk -lu my-snappy.img

Disk my-snappy.img: 4000 MB, 4000000000 bytes
4 heads, 32 sectors/track, 61035 cylinders, total 7812500 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00009745

        Device Boot      Start         End      Blocks   Id  System
my-snappy.img1   *        8192      139263       65536    c  W95 FAT32 (LBA)
my-snappy.img2          139264     2236415     1048576   83  Linux
my-snappy.img3         2236416     4333567     1048576   83  Linux
my-snappy.img4         4333568     7811071     1738752   83  Linux

```

The first partition corresponds with the boot partition (`system-boot`) and the second one contains the root read-only filesystem (`system-a`). The third partition has a copy of the root filesystem (`system-b`). This is a mechanism stablished by Canonical.
The last partition (`writable`) contains the writable content that is generally mounted on `/oem` in the file system.

For more about the Snappy Ubuntu Core file system and partitions refer to the [docs](https://developer.ubuntu.com/en/snappy/guides/filesystem-layout/).

### Mounting the images

```bash
# mount the boot partition
mkdir /mnt/system-boot
mount -o loop,offset=$(( 512 * 8192 )) my-snappy.img /mnt/system-boot
# mount the root filesystem
mkdir /mnt/system-a
mount -o loop,offset=$(( 512 * 139264 )) my-snappy.img /mnt/system-a
```
Next we’ll copy a statically built QEMU binary for ARM to the mounted image. You might need to install QEMU on the host system first. Furthermore, we need to mount or bind the special system directories from the host to the chroot.

```bash
apt-get install qemu-user-static
cp /usr/bin/qemu-arm-static /mnt/system-a/usr/bin/
mount -o bind /dev /mnt/system-a/dev
mount -o bind /proc /mnt/system-a/proc
mount -o bind /sys /mnt/system-a/sys

```

Next comes the magic. This registers the ARM executable format with the QEMU static binary. Thus, the path to qemu-arm-static has to match where it is located on the host and slave systems:

```bash
echo ':arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-arm-static:' > /proc/sys/fs/binfmt_misc/register
```

`chroot` into it:
```bash
chroot /mnt/system-a
```

```bash
umount /mnt/system-a/dev
umount /mnt/system-a/proc
umount /mnt/system-a/sys
umount /mnt/system-a
umount /mnt/system-boot
```


