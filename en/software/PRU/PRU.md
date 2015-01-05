# Getting started with the PRU

This short tutorial will explain how to get started with the Programmable Realtime Unit SubSystem using the examples provided at the [BeagleBoard PRU package](https://github.com/beagleboard/am335x_pru_package): 

First we get the [BeagleBoard PRU package](https://github.com/beagleboard/am335x_pru_package):
```
git clone git://github.com/beagleboard/am335x_pru_package.git
cd am335x_pru_package/pru_sw/app_loader/interface
make CROSS_COMPILE="" # we're compiling right on the system, so no need to cross compile
```
Now we compile `pasm`:
```
cd ../../utils/pasm_source
./linuxbuild
```
and finally some examples:
```
cd ../../example_apps
```
Here we have to change [this line](https://github.com/beagleboard/am335x_pru_package/blob/master/pru_sw/example_apps/Makefile#L8) for:
```
PASM?=../utils/pasm
```
After that small change, the examples should cross-compile **natively**:
```
make CROSS_COMPILE=""
```
Then, `cd bin`and:
```
modprobe uio_pruss #load the kernel module
echo BB-BONE-PRU-01 > /sys/devices/bone_capemgr.8/slots #activate the PRU
./PRU_memAccessPRUDataRam
```
The result should like like:
```
INFO: Starting PRU_memAccessPRUDataRam example.
        INFO: Initializing example.
        INFO: Executing example.
        INFO: Waiting for HALT command.
        INFO: PRU completed transfer.
INFO: Example executed succesfully.
```

###Sources:
- [Blog post](http://boxysean.com/blog/2012/08/12/first-steps-with-the-beaglebone-pru/)
- [am335x_pru_package/#12](https://github.com/beagleboard/am335x_pru_package/issues/12)
- [am335x_pru_package/#16](https://github.com/beagleboard/am335x_pru_package/issues/16)