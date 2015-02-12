# Ubuntu Snappy Core

### Obteniendo el kernel de Ubuntu
#### 3.16 (última)
```bash
git://kernel.ubuntu.com/ubuntu/ubuntu-utopic.git
```

##### Obteniendo el archivo de configuración
```bash
cd ubuntu-utopic
export $(dpkg-architecture -aarmhf); export CROSS_COMPILE=arm-linux-gnueabihf-
fakeroot debian/rules clean prepare-generic
```

Tu archivo debe estar disponible en `ls -al debian/build/build-generic/.config`:
```bash
victor@ubuntu:~/Desktop/ubuntu-utopic$ ls -al debian/build/build-generic/.config
-rw-rw-r-- 1 victor victor 175232 Jan 29 17:08 debian/build/build-generic/.config

```

##### Compila el código
```bash
make -j4
```

###### Compila el dts sólamente
```bash
make ARCH=arm dtbs


##### Busca el blob del árbol del dispositivo
```bash
ls arch/arm/boot/dts
```

#### 3.8 (Soporte total del kernel de BeagleBoard)
##### Obteniendo el kernel y el archivo de configuración
```bash
git clone git://kernel.ubuntu.com/ppisati/ubuntu-vivid.git
cd ubuntu-vivid/
git checkout snappy_beagle_3.8
ARCH=arm ./scripts/kconfig/merge_config.sh arch/arm/configs/erlerobot_defconfig arch/arm/configs/snappy/*.config
export CROSS_COMPILE=arm-linux-gnueabihf-
```

###### Compila el kernel
```bash
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- uImage dtbs -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- uImage-dtb.am335x-boneblack -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- modules -j4
sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_PATH=/media/victor/system-a modules_install
```
