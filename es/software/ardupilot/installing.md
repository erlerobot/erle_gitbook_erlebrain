# Instalando

- [Corriendo Debian en Erle](#corriendo-debian-en-erle)
- [Compilando el kernel de tiempo real](#compilando-el-kernel-de-tiempo-real)
- [Instalando el Kernel de tiempo real](#instalando-el-kernel-de-tiempo-real)
- [Ajustando el reloj](#ajustando-el-reloj)
- [Instalado y compilando ArduPilot](#instalado-y-compilando-ardupilot)
- [Sources](#fuentes)

El proceso se realizar utilizando un imagen de Debian, lo mismo se puede realizar con un sistema de ficheros de Ubuntu.

### Corriendo Debian en Erle
- Consigue una imagen de Debian
```
wget https://rcn-ee.net/deb/flasher/wheezy/BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz
```

- Verifica la imagen con:
```
md5sum BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz
706322a17e5f2251892ad19bec1e5829  BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz
```

- Ahora necesitas encontrar donde se ha cargado tu tarjeta microSD en el sistema de fichero (en MAC usualmente es `/dev/tty.usbserial-00002114B` y en Ubuntu `/dev/ttyUSB1`). Esto se necesita para el siguiente paso para copiar la imagen de Debian que acabamos de descarfar en la tarjeta microSD. Si no estas seguro puedes seguir estos pasos:
   - Antes de insertar tu tarjeta microSD introduce en tu ordenador `df -h`.
   - Esto listará los discos montados. Ahora introduce tu tarjeta microSD y ejecuta `df -h`
   - Compara ambas salidas y mira cual es el nuevo disco.

--------

**CUIDADO: SI ELIGES MAL EL DISPOSITO  PUEDES LIMPIAR SU DISCO DURO POR COMPLETO**

--------

- Copia la imagen de Debian en la microSD (*ejecutablo como *root*).
```
xzcat BBB-eMMC-flasher-debian-7.1-2013-10-08.img.xz | dd of=/dev/sdb bs=1M
```

- Espera hasta que el flaseo este acabado, desconecta la tarjeta microSD e introducela en **Erle**. Debian debería carga y deberías poder loguearte con *usuario: root* and *contraseña:root*.

### Compilando el kernel de tiempo real

#### Prerequisitos

----

##### ARM Compilación cruzada 
Para compilar el Kernel de Linux para la BeagleBone Black, debes tener instalado el compilador cruzado de ARM. Para instalar el compilador ejecuta:

```
apt-get install gcc-arm-linux-gnueabi
```

##### Git
Los parches de BeagleBone y los script de construcción están almacenados en un repositorio de git. Instala git:

```
apt-get install git
```

Y configuralo con tus datos
```
git config --global user.email "your.email@here.com"
```

##### Comprensión lzop
El kernel esta comprimiso utilizando `lzop`. Instala el compresor paralelo de `lzop`:
```
apt-get install lzop
```

##### uBoot mkimage
El *bootloader* usando en la BeagleBone black es *u-boot*, este tiene un formato de imagen denominado uImage. Incluye parámetros como descripciones, el tipo de maquina/arquitectura, tipo de compresión, dirección de arranque, checksum etc. Para compilar estas imágenes necesitas la herramienta `mkimage` que forma parte de la distibución u-Boot. Descarga u-boot, compila e installa las herramientas de u-boot:

```
wget ftp://ftp.denx.de/pub/u-boot/u-boot-latest.tar.bz2
tar -xjf u-boot-latest.tar.bz2
cd u-boot-2013.10   (look to see what this is called, it may have changed)
make tools
install tools/mkimage /usr/local/bin
```

----

#### Compilando el Kernel de Linux con los parches de Tiempo Real

Ahora compilamos el Kernel del BeagleBone Black, y generamos un fichero uImage añadiendo el DTB al kernel para un uso sencillo.

----

NOTA: Cuando compilamos el Kernel de Linux para Erle, el modulo de Ethernet esta desactivado para evitar problemas. Esto se puede realizar usnado la utilizadad `menuconfig`.

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

Compilamos los modulos del kernel:
```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- modules -j4
```

Y si tienes tu `rootfs` (sistema de ficheros) listo, tu puedes instalarlo (*Se asuma que tu microSD monta la partición rootfs en `/media/rootfs`*):
```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_PATH=/media/rootfs 
modules_install
```

### Instalando el Kernel de tiempo real 
#### Opción 1: Montando una tarjeta microSD
- Asegurate de que donde esta tu tarjeta microSD montada (en este caso`/dev/sdf`).
- La tarjeta microSD debe de estar montada bajo `/media` como `rootfs` (el sistema de ficheros) y `BOOT` (la partición contiene el bootloader y el kernel).
- Actualiza los módulos usando las siguiente instrucciones:

```
sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_PATH=/media/rootfs modules_install
```

- Verifica que los módulos han sido instalados apropiadamente:
```
ls /media/rootfs/lib/modules
3.12.0-rc4-armv7-x3  3.7.10-x13  3.8.13-bone28  3.8.13-rt9-00899-g40bdb63
```

verifica 3.8.13-**rt**9-00899-g40bdb63.

- Instala el nuevo kernel en la partición `BOOT` en tu microSD:
```
sudo cp arch/arm/boot/zImage /media/BOOT/zImage
```

#### Opción 2: Directamente en Erle
Una vez que has compilado el kernel puedes conectar el robot a través de un cable USB y loguarte:

```
user: root
password: root
```

Los pasos requeridos ahora están descritos [aquí](http://dev.ardupilot.com/wiki/building-for-beaglebone-black-on-linux/#Installing_the_RT_kernel).

-----

No importa que método elija, verifica la instalación después:
```
root@arm:~# uname -a
Linux arm 3.8.13-rt9-00899-g40bdb63 #1 SMP PREEMPT Wed Feb 5 16:34:58 CET 2014 armv7l GNU/Linux

```

-----

### Ajustando el reloj
Instala `cpufrequtils`:
```
apt-get install cpufrequtils
```

Luego ejecuta `cpufreq-info`:
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

Ahora edita `/etc/default/cpufrequtils` y añade lo siguiente:
```
# valid values: userspace conservative powersave ondemand performance
# get them from cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors
GOVERNOR="performance"
```

Ahora resetea (`shutdown -r 0`) y ejecuta de nuevo `cpufreq-info`:
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

### Instalado y compilando ArduPilot
Instala git, make, gawk, g++, arduino-core en tu BBB
```
apt-get install git make gawk g++ arduino-core
```

Ahora descarga el código de Ardupilot
```
git clone git://github.com/diydrones/ardupilot.git
```

Configura el código de ArduCopter:
```
cd ardupilot/ArduCopter
make configure
```

el fichero `config.mk` debe haber sido creado en el directorio de `ardupilot` (`..`). Edita este fichero de acuerdo con tus preferencias. Luego:
```
make linux
```

Compilar el código de Ardupilot en el robot toma *aproximadamente 5 minutos*. Después de que este proceso termine se creará un nuevo directorio `/tmp/ArduCopter.build/` que contiene:
```
ArduCopter.cpp  ArduCopter.elf  ArduCopter.map  libraries
ArduCopter.d    ArduCopter.lst  ArduCopter.o
```
Desde este directorio ejecuta ArduCopter.elf.

### Fuentes:
- [ArduPilot for the BeagleBone Black](http://dev.ardupilot.com/wiki/building-for-beaglebone-black-on-linux/)