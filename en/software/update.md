# Updating Erle-Brain's software

Erle-brain comes with the last available image at the time of shipping however we are continuosly developing new capabilities so eventually you might like to update it. 

### Updating the autopilot

##### Installing the tools:
````bash
sudo apt-get install gawk gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

##### Downloading the code
```bash
git clone http://github.com/erlerobot/ardupilot
```

##### Compiling the code
````bash
cd ardupilot/ArduCopter
make configure
make pxf
```

The code binaries should be deployed under `/tmp` so copy the binary into `/root` (or modify `/etc/init.d/apm4-startup.sh` and point to the binary you wish).


Generally, updating the autopilot will be enough but if you mess things up we are providing a way to reflash Erle-brain. The easiest way of doing it is using our ready-to-flash images:


### Reflashing everything on the eMMC

| Image | Date | Size | Description |
| ----------|--------|-------|------|
|[erle-debian-8-1-15.img.gz](https://mega.co.nz/#!vY8GzTTQ!pRdmdNJd1-rqdSDliD8SgKuHRrTFV_NRpxtF7p34Fhw)| 8-1-2015 | 1.34 GB | Debian, ROS Hydro (not launched at init), mavros (not launched at init), WiFi (required from the APM binary) |


Download the image and the copy it into a `>8GB microSD card`:

##### Linux
Find out the drive device (e.g. `/dev/sdb`) and copy the content of the image just downloaded:
```bash
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/sdb bs=8M
```

##### Mac OS
In Mac OS you generally need to unmount the disk before doing any kind of copying:
```bash
diskutil list
diskutil unmountDisk /dev/disk2
```
Then similarly to Linux:
```bash
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/rdisk1 bs=8m
```

##### Windows
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

### Boot from a microSD card
We also provide the option of booting directly from 
a microSD card. To do so, fetch the bootable microSD card image and 
put it into a microSD card. 

| Image | Date | Size | Description |
| ----------|--------|-------|------|
|[erle-debian-bootable-4-2-15.img.gz](https://drive.google.com/file/d/0B6D4e4nVvowdLWp0QVVIckpGUEU/view)| 8-2-2015 | 1.4 GB | Debian, ROS Hydro, mavros (launched at init), WiFi (required from the APM binary) |
| [erle-snappy-3-3-15.img.gz](https://mega.co.nz/#!SEdWGQII!8g8-vZqh1H0drqlVQXvIX1HYcOTarp1QR0jAfm6HsPo) | 3-3-15 | 1.47 GB | Snappy Ubuntu Core for [Erle-Brain](erlerobotics.com/blog/product/erle-brain), includes APM ("apm" service), ROS Indigo preinstalled and launched at init ("ros" service), mavros presintalled (available for superuser) |
|[erle-snappy-17-3-15.img.gz](https://mega.co.nz/#!fBMzAYgI!9GTMQEAbokBlVMSzhkbdj_C2WJtmK5dkmS9Mp8j1Wpc) | 17-3-15 | 1.51 GB | Snappy Ubuntu Core for [Erle-Brain](erlerobotics.com/blog/product/erle-brain), includes APM ("apm" service), ROS Indigo preinstalled and launched at init ("ros" service), mavros presintalled (available for superuser), ROS packages preinstalled as well, APM:Plane and APM:Copter apps installed (only copter running by default)  |
|[erle-snappy-25-3-15.img.gz](https://mega.co.nz/#!uckwnSqT!f_UBsgstZXjnq2cck3M3X9qHRoD2dQbtIq1Ykp8RLFo) | 25-3-15 | 1.51 GB | Snappy Ubuntu Core for [Erle-Brain](erlerobotics.com/blog/product/erle-brain) (ROS, and some nodes launched at init)  |
|[erle-snappy-23-4-15.img.gz](erle_gitbook_erlebrain) | 23-4-15 | 1.74 GB | Snappy Ubuntu Core for [Erle-Brain](erlerobotics.com/blog/product/erle-brain) (ROS Indigo, and some nodes launched at init, telemetry support via WiFi, USB and/or Telemetry Radio)  |


Place it on Erle-Brain and start playing with it :). Customizations can be made editing `/etc/init.d/apm4-startup.sh`.


