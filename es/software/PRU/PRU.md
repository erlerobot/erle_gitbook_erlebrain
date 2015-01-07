# Comenzando con PRU

Este pequeño tutorial explica como comenzar con la Unidad Programable de Tiempo Real usando los ejemplos proporcionados en [BeagleBoard PRU package](https://github.com/beagleboard/am335x_pru_package): 

Lo primero cogemos el [paquete BeagleBoard PRU](https://github.com/beagleboard/am335x_pru_package):

```
git clone git://github.com/beagleboard/am335x_pru_package.git
cd am335x_pru_package/pru_sw/app_loader/interface
make CROSS_COMPILE="" # we're compiling right on the system, so no need to cross compile
```

Ahora compilamos `pasm`:
```
cd ../../utils/pasm_source
./linuxbuild
```

y finalmente algunos ejemplos:
```
cd ../../example_apps
```

Ahora cambiamos esta [línea](https://github.com/beagleboard/am335x_pru_package/blob/master/pru_sw/example_apps/Makefile#L8) por:
```
PASM?=../utils/pasm
```

Después de este pequeño cambio, se puede compilar, se puede compilar de fomra cruzada **nativamente**:
```
make CROSS_COMPILE=""
```

Luego, `cd bin` y:
```
modprobe uio_pruss #load the kernel module
echo BB-BONE-PRU-01 > /sys/devices/bone_capemgr.8/slots #activate the PRU
./PRU_memAccessPRUDataRam
```

El resultado se parece a:
```
INFO: Starting PRU_memAccessPRUDataRam example.
        INFO: Initializing example.
        INFO: Executing example.
        INFO: Waiting for HALT command.
        INFO: PRU completed transfer.
INFO: Example executed succesfully.
```

### Fuentes:
- [Blog post](http://boxysean.com/blog/2012/08/12/first-steps-with-the-beaglebone-pru/)
- [am335x_pru_package/#12](https://github.com/beagleboard/am335x_pru_package/issues/12)
- [am335x_pru_package/#16](https://github.com/beagleboard/am335x_pru_package/issues/16)