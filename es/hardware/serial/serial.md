# Com obtener una conexión serial a través de USB 

### Linux or Mac OS

Los sistemas operativos actuales incluyen los drives FTDI por defecto, por lo tanto, crear una conexión serial es tan fácil como:

- Conecta un cable miniUSB
- Si todo esta instalado, dos nuevos dispositivos aparecerán bajo **/dev**: `/dev/ttyUSB0` and `/dev/ttyUSB1`.
- Lanza una conexión serial a través de `/dev/ttyUSB1` con:

```
screen /dev/ttyUSB1 115200
```

- Si todo es correcto (asumiendo que la tarjeta SD no esta conectado) deberías ver algunas "C"s imprimidas en la pantalla:

```
CCCCC
```

*(también puede usar [minicom](http://en.wikipedia.org/wiki/Minicom) instead of screen).*


### Windows 7

####Instalando los drivers
- [BONE_D64.exe (64 bits)](http://beagleboard.org/static/beaglebone/latest/Drivers/Windows/BONE_D64.exe) 
- [BONE_DRV.exe (32 bits)](http://beagleboard.org/static/beaglebone/latest/Drivers/Windows/BONE_DRV.exe)

#### Buscando el puerto serial virual de Erle

- Inicio > Dispositicos e impresoras
- Busca por BeagleBone/XDS 100v2
- Doble click o click izquierdo 
- Clicka en la pestaña de *Hardware*
- Debajo de “Device Functions” hay una lista de puertos con el tipo de puerto en la siguiente columna.
- Uno de ellos tiene el título de “USB Serial Port (COMn)”. En mi caso ”USB Serial Port (COM5)”

#### Obteniendo y ejecutando un terminal 
Para este proposito se utiliza [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/). PuTTY es un único fichero `.EXE`. No tiene instalador. He creado un directorio denominado `C:\Program Files (x86)\putty`.

Descagar `putty.exe`y copialo en el directorio. Luego haz click-izquierdo en `putty.exe`y elige la opción “Pin to Start Menu” para hacerlo más sencillo.

#### Configura el terminal para usar una conexión SSH a trávés de una conexión serie:
- Lanzar Putty. Es un programa simple y en la ventana principal hay un menu denominado “PuTTY Configuration”
- Asegurate que la conexción serial esta seleccionada. Y la configuración de puerto serie aparecerá
- Debajo de “Serial line” introduce el número del puerto COM que has descubierto anteriormente, `COM5` es este caso:
- Introduce 115200 en el capo de velocidad
- Pulsa el boton *Open*.
- Ahora resetea el robot (pulsado el botón de reset) y deberías de ver:
   - Si no hay una tarjeta microSD introducida "CCCC.." 
   - Si hay una tarjeta microSD introducida el booteo del kernel.