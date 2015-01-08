# Installing

This process will be done using a prebuilt Debian image, the same could be done with a Ubuntu FS.

- [Get Debian running on Erle](#get-debian-running-on-erle)
- [Making the rt kernel](#making-the-rt-kernel)
- [Installing the RT kernel](#installing-the-rt-kernel)
- [Adjusting the clock](#adjusting-the-clock)
- [Installing and Making ArduPilot](#installing-and-making-ardupilot)
- [Sources](#sources)

###Get Debian running on Erle

- Get a Debian image:
```
wget https://rcn-ee.net/deb/flasher/wheezy/BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz
```
- Verify Image with:
```
md5sum BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz
706322a17e5f2251892ad19bec1e5829  BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz
```

- Now you need to find out where your microSD card is being loaded in the File System (in Mac OS it's usually `/dev/tty.usbserial-00002114B` and in Ubuntu `/dev/ttyUSB1`). This is needed in the following step to copy the Debian image that we just downloaded into the microSD card. If you are not sure you can follow these simple steps:
   - Before plugging your SD card into your computer, type `df -h`.
   - This will list the current mounted drives. Now plug in your SD card and type: `df -h`
   - Compare both outputs and see which is the new drive. 

--------

**WARNING: IF YOU GET THIS WRONG   YOU CAN WIPE YOUR HARD DISK**

--------
- Copy the Debian image to the microSD (*executed as root on this form*):
```
xzcat BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz | dd of=/dev/sdb bs=1M
```

- Wait until the flashing is finished, unplug the microSD card and plug it into **Erle**. Debian should load and you should be able to log in with *user: root* and *password:root*.

### Making the rt kernel

#### Prerequisites

----

##### ARM Cross Compiler
To compile the linux kernel for the BeagleBone Black, you must first have an ARM cross compiler installed. To install the compiler run:
```
apt-get install gcc-arm-linux-gnueabi
```

##### git
The BeagleBone patches and build scripts are stored in a git repository. Install git:
```
apt-get install git
```
And configure with your identity.
```
git config --global user.email "your.email@here.com"
```

##### lzop Compression
The kernel is compressed using `lzop`. Install the `lzop` parallel file compressor:
```
apt-get install lzop
```
##### uBoot mkimage
The bootloader used on the BeagleBone black is u-boot. *u-boot* has a special image format called uImage. It includes parameters such as descriptions, the machine/architecture type, compression type, load address, checksums etc. To make these images, you need to have a `mkimage` tool that comes part of the u-Boot distribution. Download u-boot, make and install the u-boot tools:
```
wget ftp://ftp.denx.de/pub/u-boot/u-boot-latest.tar.bz2
tar -xjf u-boot-latest.tar.bz2
cd u-boot-2013.10   (look to see what this is called, it may have changed)
make tools
install tools/mkimage /usr/local/bin
```

----

#### Compiling the Linux Kernel with the RT patch
Here we compile the BeagleBone Black Kernel, and generate an uImage file with the DTB blob appended to the kernel for ease of use.

----

NOTE: When making the Linux kernel for Erle, the Ethernet module should be deactivated to avoid problems. This can be done using the `menuconfig` utility.

----

```
git clone git://github.com/beagleboard/kernel.git
cd kernel
git checkout 3.8-rt
./patch.sh
cp configs/beaglebone kernel/arch/arm/configs/beaglebone_defconfig
wget http://arago-project.org/git/projects/?p=am33x-cm3.git\;a=blob_plain\;f=bin/am335x-pm-firmware.bin\;hb=HEAD -O kernel/firmware/am335x-pm-firmware.bin
cd kernel
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- beaglebone_defconfig -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- uImage dtbs -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- uImage-dtb.am335x-boneblack -j4
```
Now we build any kernel modules:
```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- modules -j4
```
And if you have your rootfs ready, you can install them (*this is assuming that your microSD card mounts the rootfs partition at `/media/rootfs`*):
```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_PATH=/media/rootfs 
modules_install
```

### Installing the RT kernel
#### Option 1: Mounting the microSD card in the build machine
- Figure out where your microSD was mounted (mine at `/dev/sdf`).
- The microSD card should also be mounted under `/media` as `rootfs` (the filesystem) and `BOOT` (the boot partition containing the bootloader and the kernel).
- Update the modules using the following instruction:
```
sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_PATH=/media/rootfs modules_install
```
- Verify that the modules have been installed properly:
```
ls /media/rootfs/lib/modules
3.12.0-rc4-armv7-x3  3.7.10-x13  3.8.13-bone28  3.8.13-rt9-00899-g40bdb63
```
3.8.13-**rt**9-00899-g40bdb63 verifies it.
- Install the new kernel into the `BOOT` partition of your microSD:
```
sudo cp arch/arm/boot/zImage /media/BOOT/zImage
```

#### Option 2: Directly on Erle
Once we've finished making the kernel we can connect the robot to the USB cable and log in:
```
user: root
password: root
```

The steps required now are described [here](http://dev.ardupilot.com/wiki/building-for-beaglebone-black-on-linux/#Installing_the_RT_kernel).


-----

No matter which method you choose, verify your installation afterwards:
```
root@arm:~# uname -a
Linux arm 3.8.13-rt9-00899-g40bdb63 #1 SMP PREEMPT Wed Feb 5 16:34:58 CET 2014 armv7l GNU/Linux

```

-----

### Adjusting the clock
Install `cpufrequtils`:
```
apt-get install cpufrequtils
```
Then execute `cpufreq-info`:
```
cpufreq-info 
cpufrequtils 007: cpufreq-info (C) Dominik Brodowski 2004-2009
Report errors and bugs to cpufreq@vger.kernel.org, please.
analyzing CPU 0:
  driver: generic_cpu0
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency: 300 us.
  hardware limits: 275 MHz - 720 MHz
  available frequency steps: 275 MHz, 500 MHz, 600 MHz, 720 MHz
  available cpufreq governors: conservative, ondemand, userspace, powersave, performance
  current policy: frequency should be within 275 MHz and 720 MHz.
                  The governor "ondemand" may decide which speed to use
                  within this range.
  current CPU frequency is 275 MHz (asserted by call to hardware).
  cpufreq stats: 275 MHz:nan%, 500 MHz:nan%, 600 MHz:nan%, 720 MHz:nan%

```
Now edit `nano /etc/default/cpufrequtils` and add the following:
```
# valid values: userspace conservative powersave ondemand performance
# get them from cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors
GOVERNOR="performance"
```
Now reboot (`shutdown -r 0`) and run again `cpufreq-info`:
```
cpufreq-info 
cpufrequtils 007: cpufreq-info (C) Dominik Brodowski 2004-2009
Report errors and bugs to cpufreq@vger.kernel.org, please.
analyzing CPU 0:
  driver: generic_cpu0
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency: 300 us.
  hardware limits: 275 MHz - 720 MHz
  available frequency steps: 275 MHz, 500 MHz, 600 MHz, 720 MHz
  available cpufreq governors: conservative, ondemand, userspace, powersave, performance
  current policy: frequency should be within 275 MHz and 720 MHz.
                  The governor "performance" may decide which speed to use
                  within this range.
  current CPU frequency is 720 MHz (asserted by call to hardware).
  cpufreq stats: 275 MHz:nan%, 500 MHz:nan%, 600 MHz:nan%, 720 MHz:nan%

```

### Installing and Making ArduPilot
Install git, make, gawk, g++, arduino-core on your BBB
```
apt-get install git make gawk g++ arduino-core
```
Now get the ArduPilot code (43.40 MB):
```
git clone git://github.com/diydrones/ardupilot.git
```
Configure the ArduCopter code:
```
cd ardupilot/ArduCopter
make configure
```
a file `config.mk` should have been created at the `ardupilot` directory (`..`). Edit this file according to your preferences. Then:
```
make linux
```
Compiling ArduPilot in the robot takes *approximately 5 minutes*. After the process has finished you will have a new directory `/tmp/ArduCopter.build/` containing:
```
ArduCopter.cpp  ArduCopter.elf  ArduCopter.map  libraries
ArduCopter.d    ArduCopter.lst  ArduCopter.o
```
from this directory, run the ArduCopter.elf.

###Sources:
- [ArduPilot for the BeagleBone Black](http://dev.ardupilot.com/wiki/building-for-beaglebone-black-on-linux/)