# Updating Erle-Brain's software

Erle-brain comes with the last available image at the time of shipping however we are continuosly developing new capabilities so eventually you might like to update it. 

---

**We expect to automate this soon**.

---

### Updating the autopilot

#### Installing the tools:
````bash
sudo apt-get install gawk gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

#### Downloading the code
```bash
git clone http://github.com/erlerobot/ardupilot
```

#### Compiling the code
````bash
cd ardupilot/ArduCopter
make configure
make pxf
```

The code binaries should be deployed under `/tmp` so copy the binary into `/root` (or modify `/etc/init.d/apm4-startup.sh` and point to the binary you wish).


### Reflashing everything on the eMMC

Generally, updating the autopilot will be enough but if you mess things up we are providing a way to reflash Erle-brain. The easiest way of doing it is using our [ready-to-flash eMMC last image](https://mega.co.nz/#!aQ8FnB4B!CpqMmZdVyOWvryxdb9Hzvo2UnL44L-0JttPRswgC6Ek).

Download the image and the copy it into a `>8GB microSD card`:

#### Linux
Find out the drive device (e.g. `/dev/sdb`) and copy the content of the image just downloaded:
```bash
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/sdb bs=8M
```

#### Mac OS
In Mac OS you generally need to unmount the disk before doing any kind of copying:
```bash
diskutil list
diskutil unmountDisk /dev/disk2
```
Then similarly to Linux:
```bash
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/rdisk1 bs=8m
```

#### Windows
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
