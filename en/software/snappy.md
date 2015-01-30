# Ubuntu Snappy Core

### Getting the Ubuntu kernel
```bash
git://kernel.ubuntu.com/ubuntu/ubuntu-utopic.git
```

### Getting the configuration file
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

### Compile the code
```bash
make -j4
```

#### Compile the dts solely
```bash
make ARCH=arm dtbs
```

### Find the device tree blobs
```bash
ls arch/arm/boot/dts
```