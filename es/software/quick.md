# Comienzo rápido con Erle-Brain

### Conectando a Erle-Brain
Hay varias formas de conectarse a Erle-Brain:

##### Usando un dongle WiFi (Frecuencia de 5 GHz)
Si has adquirido tu cerebro vía WiFi, verás que hemos adjuntado y configurado el dongle que creará automáticamente una red WiFi (modo hotspot), con nombres con el prefijo `erle`. Normalmente la dirección IP de Erle-Brain es `11.0.0.1`, u tu máquina debería conseguir la IP `11.0.0.2` (el servidor DHCP ha sido configurado para asignar una sola dirección IP).

----

*Ten en cuenta que tu portatil/teléfono/tableta/... soporta los 5 GHz. Deberías mirar un soporte de AC para 802.11.**

----

----

Erle-Brain crea una red llamada "erle-something" o "erle".
La contraseña para la red es **holaerle**.

-----

##### A través de un mini USB
Erle-Brain soporta un modo cliente USB. Usando este mecanismo de conexión y el módulo de kernel Ethernet-over-USB deberías poder conectarte vía SSH en la placa.

Para hacerlo, conecta Erle-Brain usando un conector mini USB:

![](https://erlerobotics.com/blog/wp-content/uploads/2014/12/IMG_6334.jpg)

Busca la nueva interfaz de red que debe haber sido creada en tu sistema operativo y asigna la siguiente dirección IP: `192.168.7.1`. Suponiendo que tu nueva interfaz es eth6 y estás en Linux:
```bash
sudo ifconfig eth6 192.168.7.1
```

Ahora que estás en la misma subred, conecta SSH en la placa:
```bash
ssh root@192.168.7.2
```

##### Usando un GCS (MissionPlanner, APMPlanner, DroidPlanner, etc)
Selecciona `UDP` y usa la dirección IP `11.0.0.1` y el puerto `6000`.

### Comprobando el proceso
```bash
ps -e
```
Ahora deberías ver `Arducopter.elf` y `apm4-startup.sh` trabajando.

### Cambiando a otro vehículo
Tengamos en cuenta que queremos usar Erle-Brain con el plane:
```bash
cd ardupilot/ArduPlane
make configure
make pxf
# Esto creará un binario llamado ArduPlane.elf en ~/build
cp ~/ArduPlane.elf ~/
```

Ahora deberías actualizar los guiones de comienzo para lanzar el plane dentro de copter. Para hacerlo, edita el archivo `/etc/init.d/apm4-startup.sh` y al final de el archivo, modifica la llamada de lanzamiento de ArduCopter para que sea en ArduPlane.

### Lanzando el ROS
```bash
roscore &
```

Ahora comprueba que los nodos y los tópicos están disponibles:
```bash
rosnode list
rostopic list
```
