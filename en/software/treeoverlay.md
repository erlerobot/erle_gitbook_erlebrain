# Check the device tree overlays

Device Tree Overlays allow the device tree that was accessed by the kernel at boot to be modified afterwards in *userspace*. If a new piece of hardware like a cape is added to BBB or the onboard header pins are initialized/re-configured an overlay can enable the modifications without having to reboot. On the default Angstrom Linux that ships with BBB there is a bunch of DT overlays already created and available for use in `/lib/firmware`.

A cape manager has been implemented to load and (in theory) unload overlays and slots is its interface and can show us what is currently loaded. To check the currently loaded overlays:
```
cat /sys/devices/bone_capemgr.8/slots
```
```
 0: 54:PF---                                                                                                                                          
 1: 55:PF---                                                                                                                                          
 2: 56:PF---                                                                                                                                          
 3: 57:PF---       
```

The first four are reserved for hardware capes while the software ones can be loaded without limit (theoretically):

```
 0: 54:PF---                                                                                                                                          
 1: 55:PF---                                                                                                                                          
 2: 56:PF---                                                                                                                                          
 3: 57:PF---                                                                                                                                          
 4: ff:P-O-L Override Board Name,00A0,Override Manuf,am33xx_pwm                                                                                       
 5: ff:P-O-L Override Board Name,00A0,Override Manuf,bone_pwm_P9_16                                                                                   
 6: ff:P-O-L Override Board Name,00A0,Override Manuf,bone_pwm_P8_13                                                                                   
 7: ff:P-O-L Override Board Name,00A0,Override Manuf,bone_pwm_P9_21                                                                                   
 8: ff:P-O-L Override Board Name,00A0,Override Manuf,bone_pwm_P9_28         
```
