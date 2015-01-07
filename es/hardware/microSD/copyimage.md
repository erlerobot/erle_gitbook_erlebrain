#Copiar una imagen en la tarjeta microSD

## Linux

Los siguiente pasos describen como **copiar una image en una tarjeta microSD**. La imagen usualmente contiene el *bootloader*, el *Kernel de Linux* y el **sistema de ficheros* (como Ångström o Ubuntu otros son posibles) de **Erle robot*.

   * Consigue la imagen.
   * Asumiendo que el nombre de la imagen como `erlerobot-ubuntu.img.gz`, y tu microSD esta montado bajo `/dev/sdb` (usualmente bajo Linux) realiza los siguiente en el terminal Linux:

```
zcat erlerobot-ubuntu.img.gz | sudo dd of=/dev/sdb bs=8M
```

*(Nota en Mac OS `8M` debe de ser sustituido por `8m`)*

   * Para asegurarse de que todo el contenido ha sido copiado en la microSD, introduce en el terminal.
```
sync
```
   * Extrae la tarjeta microSD y conetarla a Erle. Ya estas listo para volar ;).
------------

Alternativamente si tu imagen es del tipo `erlerobot-ubuntu.tar.xz` debes ejecutar el siguiente comando:

```
xz -dkc erlerobot-ubuntu.tar.xz > /dev/sdb
```

## En MacOS

```
diskutil list
diskutil unmountDisk /dev/disk2
```

```
sudo dd if=~/Downloads/erle-debian-11-12-14.img of=/dev/rdisk2 bs=4m conv=sync,notrunc,noerror   
```

------------

## En Windows
PAra quemar una imahen en un tarjeta microSD necesitas las siguiente herramientas:

- [7-zip](http://www.7-zip.org/) o algún descompresor que pueda extraer ficheros gzipped (extensión gz).
- [Image Writer](https://launchpad.net/win32-image-writer) para Windows para escribir el fichero `.img` en la tarjeta microSD.
- Un ordenador con una tarjeta miscroSD, o un adaptador USB externo.

Aquí el procedimiento para preparar la tarjeta microSD.

- Descarga la imagen de Ubuntu en formato gzipped.
- Extrae el `.img` usando 7-zip.
- Insertar la tarjeta en el lector.
- Executar Win32DiskImager.exe - requiere privilegios de administrador en Windows 7.
- Selecciona la imagen.
- Selecciona el dispositivo correspondiente a la tarjeta SD. 
- Escribe la image. Tomará un tiempo.
- Extrae la tarjeta SD