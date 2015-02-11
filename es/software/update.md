# Actualizando el software de Erle-Brain

Erle-brain viene con la última imágen disponible a la hora de envío, sin embargo, estamos desarrollando continuamente nuevas prestaciones, así que de vez en cuando, deberías acualizarlo.

---

**Esperemos que ésto se automatize pronto**.

---

## Actualizando el autopiloto

### Instalando las herramientas:
````bash
sudo apt-get install gawk gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

### Descargando el código
```bash
git clone http://github.com/erlerobot/ardupilot
```

### Compilando el código
````bash
cd ardupilot/ArduCopter
make configure
make pxf
```

Los binarios deben de estar bajo `/tmp` así que copia el binario a `/root` (o modifica `/etc/init.d/apm4-startup.sh` y apunta hacia el binario deseado).


### Reflasheando todo en el eMMC

Normalmente, actualizar el autopiloto debe ser suficiente, pero si estropeas algo, estamos proporcionando una forma de reflashear Erle-Brain. La forma mas fácil de hacerlo es usando nuestro [ready-to-flash eMMC last image](https://mega.co.nz/#!aQ8FnB4B!CpqMmZdVyOWvryxdb9Hzvo2UnL44L-0JttPRswgC6Ek).

Descarga la imagen y copiala en `>8GB microSD card`:

#### Linux
Encuentra la unidad del dispositivo (ej. `/dev/sdb`) y copia el contenido de la imágen que acabas de descargar:
```bash
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/sdb bs=8M
```

#### Mac OS
En Mac OS normalmente tienes que desmontar el disco antes de hacer cualquier tipo de copia:
```bash
diskutil list
diskutil unmountDisk /dev/disk2
```
Después similar a Linux:
```bash
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/rdisk1 bs=8m
```

#### Windows
Para quemar una imagen en una tarjeta SD, en Windows primero necesitas las siguientes herramientas:
- [7-zip](http://www.7-zip.org/) u otro descompresor que extraiga archivos con extensión gz.
- [Image Writer](https://launchpad.net/win32-image-writer) para Windows, para escribir la imagen en la Tarjeta SD.
- A laptop with SD Card reader, or an external USB SD Card reader.
- Un laptop con lector de tarjetas SD, o un lector USB de tarjetas extraíble.

Aquí está el procedimiento para preparar la tarjeta SD

- Descarga la imagen Ubuntu de extensión gzip para las placas OMAP4.
- Extrae la imagen usando 7-zip.
- Inserta la tarjeta SD en el lector.
- Ejecuta Win32DiskImager.exe - te solicitará permisos de administrador en Windows 7.
- Selecciona la imagen.
- Selecciona el dispositivo que corresponde al lector de la tarjeta SD.
- Escribe la imagen. Esto llevará un rato.
- Extrae la tarjeta SD.

### Arranque desde la tarjeta microSD.¡
También proporcionamos la opción de arrancar directamente desde la tarjeta microSD. Para hacer esto, coje la [bootable microSD card image](https://drive.google.com/file/d/0B6D4e4nVvowdLWp0QVVIckpGUEU/view) e introducela en la tarjeta microSD. Sitúala en Erle-Brain y comienza con él ;).

Esta imagen contiene:

 - Sistema de archivos Debian Wheezy
 - APM
 - ROS Hydromedusa instalado e iniciado en init
 - El paquete mavros ROS iniciado en init

La personalización es posible editando `/etc/init.d/apm4-startup.sh`. 
