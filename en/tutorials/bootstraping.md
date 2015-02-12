# Bootstraping and image for Erle-Brain

Image you'd like to create a File System image to develop for Erle-Brain in your local machine. How would you do that without having to start from one of our own images.

That's exactly what would be covered here.

### Creating the image
```bash
mkdir ~/vivid
qemu-debootstrap --arch=armhf vivid ~/vivid/
```

### Mounting the images

```bash
apt-get install qemu-user-static
sudo cp /usr/bin/qemu-arm-static ~/vivid/usr/bin/
sudo mount -o bind /dev ~/vivid/dev
sudo mount -o bind /proc ~/vivid/proc
sudo mount -o bind /sys ~/vivid/sys

```

Next comes the magic. This registers the ARM executable format with the QEMU static binary. Thus, the path to qemu-arm-static has to match where it is located on the host and slave systems:

```bash
echo ':arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-arm-static:' > /proc/sys/fs/binfmt_misc/register
```

`chroot` into it:
```bash
chroot /mnt/system-a
```