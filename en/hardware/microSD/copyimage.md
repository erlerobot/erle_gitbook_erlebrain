#Copy image to microSD card

## Linux

The following steps describe how to **copy an image to the microSD card**. The usually will contain the *bootloader*, the *Linux Kernel* and a *File System* (either Ångström or Ubuntu, others are possible) for the **robot Erle**:

   * Fetch the image.
   * Assuming the image name is `erlerobot-ubuntu.img.gz`, and your microSD is mounted under `/dev/sdb` (usually is like this in Linux) do the following in the Linux terminal:

```
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/sdb bs=8M
```

*(Note: in Mac OS the `8M` should be substituted by `8m`)*


   * In order to make sure that all the content has been copied to the microSD card, introduce in the terminal:

```
sync
```
   * Eject your microSD card and connect it to Erle. It should be ready to fly ;).

------------

Alternatively if you have an image of the kind `erlerobot-ubuntu.tar.xz` then you should use the following command:
```
xz -dkc erlerobot-ubuntu.tar.xz > /dev/sdb
```

## Do it in MacOS

```
diskutil list
diskutil unmountDisk /dev/disk2
```

```
sudo dd if=~/Downloads/erle-debian-11-12-14.img of=/dev/rdisk2 bs=4m conv=sync,notrunc,noerror   
```

------------

## Do it in Windows
To burn an image to an SD card in windows you first need the following tools:
- [7-zip](http://www.7-zip.org/) or other decompressor that can extract gzipped file (extension gz).
- [Image Writer](https://launchpad.net/win32-image-writer) for Windows to write the img file to the SD Card.
- A laptop with SD Card reader, or an external USB SD Card reader.

Here’s the procedure to prepare the SD Card

- Download Ubuntu gzipped image file for OMAP4 boards.
- Extract img file using 7-zip.
- Insert SD Card into reader.
- Execute Win32DiskImager.exe – it requests admin privileges on Windows 7.
- Select the image file.
- Select the device that corresponds to the SD Card reader.
- Write the image file. This will take a while.
- Eject the SD Card.
