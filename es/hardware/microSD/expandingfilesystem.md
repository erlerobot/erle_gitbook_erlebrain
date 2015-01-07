#Expandiendo el sistema de ficheor en la microSD

Generalmente los dispotivos de almacenamiento necesitan una estructra que permita al sistema opertativo localizar y crear nuevas ficheros. Esto se hace usando particiones y el sistema de ficheros. Una partición es una sección del sistema de almacenamiento que esta formateado con el sistema de fichero, en el cúal el sistema operativo crea una estructura de directorios. Expandiendo el sistema de ficheros en la microSD se puede utilizar el resto de la memoria para este proposito.
Los pasos para expandir el sistema de ficheros en una tarjeta microSD externa:

- Introduce la tarjeta microSD en el robot, arranca y localiza tu partición. `lsblk` y `df -h` te puede ayudar a ello:

```
$lsblk 
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
mmcblk0     179:0    0   7.5G  0 disk 
|-mmcblk0p1 179:1    0    48M  0 part /boot/uboot
`-mmcblk0p2 179:2    0   7.4G  0 part /
```

```
$df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/mmcblk0p2  7.2G  3.3G  3.6G  49% /
devtmpfs        122M  4.0K  122M   1% /dev
none             25M  200K   25M   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            122M     0  122M   0% /run/shm
/dev/mmcblk0p1   48M  9.4M   39M  20% /boot/uboot
```
- Para manejar las particiones del disco utiliza el comando `fdisk`:
```
$fdisk /dev/mmcblk0
```

Luego introduce `p` esto debería de ser el tamaño de tu tarjeta SD. Dependiendo del tamaño de la SD, el valor puede cambiar.
```
$fdisk /dev/mmcblk0

Command (m for help): p

Disk /dev/mmcblk0: 8010 MB, 8010072064 bytes
4 heads, 16 sectors/track, 244448 cylinders, total 15644672 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

        Device Boot      Start         End      Blocks   Id  System
/dev/mmcblk0p1   *        2048      100351       49152    e  W95 FAT16 (LBA)
/dev/mmcblk0p2          100352    15523839     7711744   83  Linux
```
 - Introduce el comando `d` para eliminar la partición y denomina la partición con el número `2`. Introduce `p` de nuevo para mostrar que ha sido elimiando `/dev/mmcblk0p2`

   - Ahora crea una nueva partición introduciendo `n` luego `p` y finalmente `2`

   - Now create a new partition by entering ‘n’ then ‘p’ and then ‘2’

   - n–> nueva partición

   - p–>primaria (tipo de partición)

   - 2–> numero de partición

- Puedes introducir `p` de nuevo para ver que `/dev/mmcblk0p2` ha aparecido de nuevo.

- Si todo ha ido correctamente introduce `w` para realizar los cambios o si has incontrado algún error pulsa `Ctrl-Z` para parar el proceso.

- Reinicia el sistema.
```
$ reboot
```

- Después de reiniciar necesitas expandir el sistema de ficheros. Introduce el comando `df` (df-> muestra el espacio total y el espacio libre disponible en el sistema).

- Luego introduce el siguiente comando para extender el sistema de ficheros. (requiere de tiempo para completarse).

```
$ resize2fs /dev/mmcblk0p2      
```

Después de completarse el proceso ejecuta de nuevo `df` para chequear el espacio disponible. Ahora puedes acceder a todo el espacio de tu microSD.

### Fuente:
- [Expanding the File System Partition on a SD card in BeagleBone Black](http://blogspot.tenettech.com/?p=2932)