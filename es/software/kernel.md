# Linux kernel

El kernel de Linux no ha sido diseñado para cumplir restricciones de tiempo real, sim embargo hay algunas técnicas que pueden ayudar a ajustar el kernel al cumplimiento de ciertos plazos. Cuatro nucleos han sido evaluados (algunos disponibles [aquí](https://github.com/erlerobot/kernels/tree/master/bbb/3.8.13.x)):

Las siguientes pruebas se realizarán con un kerneel vanilla compilado con la opción `PREEMPT`. Se hará uso del código de [cylictests](https://rt.wiki.kernel.org/index.php/Cyclictest) y también vamos a torturar al kernel con `stress` para obtener las latencias máximas. 



### Toruturando el kernel 

Para este proposito vamos a utilizar [stress](http://linux.die.net/man/1/stress) Linux command line tool.

Mientras se ejecuta el autopiloto y usando:
``` bash
stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
```

Se puede observar que la carga del sistema es `17` aproximadamente:
```bash
root@beaglebone:~# uptime
 03:20:20 up 14 min,  3 users,  load average: 17.07, 7.86, 3.21
```

Vamos a resivar más profundamente los hilos y las operaciones de entrada salida:
```bash
stress --cpu 64 --io 64 --vm 2 --vm-bytes 128M
```
Con esto, el comando `uptime` devuelve:
```bash
root@beaglebone:~# uptime
 03:27:10 up 21 min,  3 users,  load average: 129.08, 81.93, 36.63
```

### Ejecutando cyclictests
Ahora que hemos extresado el procesador se utilizará `cyclictests` para medir las latencias (us):
```bash
root@beaglebone:~# cyclictest -t1 -p 80 -n -i 10000 -l 10000
# /dev/cpu_dma_latency set to 0us
policy: fifo: loadavg: 0.47 0.65 0.44 1/98 1312           

T: 0 ( 1307) P:80 I:10000 C:   3892 Min:     19 Act:   26 Avg:   67 Max:   13743
```

