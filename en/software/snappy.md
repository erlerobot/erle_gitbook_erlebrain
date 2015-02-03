# Ubuntu Snappy Core

### Getting the Ubuntu kernel
#### 3.16 (latest)
```bash
git://kernel.ubuntu.com/ubuntu/ubuntu-utopic.git
```

##### Getting the configuration file
```bash
cd ubuntu-utopic
export $(dpkg-architecture -aarmhf); export CROSS_COMPILE=arm-linux-gnueabihf-
fakeroot debian/rules clean prepare-generic
```

Your file will be available at `ls -al debian/build/build-generic/.config`:
```bash
victor@ubuntu:~/Desktop/ubuntu-utopic$ ls -al debian/build/build-generic/.config
-rw-rw-r-- 1 victor victor 175232 Jan 29 17:08 debian/build/build-generic/.config

```

##### Compile the code
```bash
make -j4
```

###### Compile the dts solely
```bash
make ARCH=arm dtbs
```

##### Find the device tree blobs
```bash
ls arch/arm/boot/dts
```

#### 3.8 (BeagleBoard kernel full support)
##### Getting the kernel, the configuration file and compiling it
```bash
git clone git://kernel.ubuntu.com/ppisati/ubuntu-vivid.git
cd ubuntu-vivid/
git checkout snappy_beagle_3.8
ARCH=arm ./scripts/kconfig/merge_config.sh arch/arm/configs/erlerobot_defconfig arch/arm/configs/snappy/*.config
export CROSS_COMPILE=arm-linux-gnueabihf-
make -j4
```
