# Systemd

### Primeros pasos `systemd`

>systemd es un gestor del sistema y de servicios para Linux, compatible con los scripts init SysV y LSB. systemd proporciona una paralelización agresiva de prestaciones, usa un socket y una activación D-Bus para los servicios entrantes, ofrece una bajo demanda del los daemons entrantes, guarda los registros de procesos usando los grupos de control de Linux, soporta instantáneas y restauraciones del estado del sistema, mantiene puntos de montaje y desmontaje e implementa una elaboración transaccional basada en la dependencia del servicio de control de lógica.
>
> https://wiki.archlinux.org/index.php/Systemd

Con `systemd` tenemos un directorio `/etc/systemd/system/` lleno de enlaces simbólico a archivos en `/lib/systemd/system/`. `/lib/systemd/system/` contiene scripts init; para empezar un servicio en el arranque debe estar unido a `/etc/systemd/system/`. El comando systemctl hace esto cuando activas un nuevo servicio.