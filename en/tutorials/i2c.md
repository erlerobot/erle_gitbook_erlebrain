#How to instantiate I2C devices from the userspace

> In general, the kernel should know which I2C devices are connected and          
> what addresses they live at. However, in certain cases, it does not, so a       
> sysfs interface was added to let the user provide the information. This         
> interface is made of 2 attribute files which are created in every I2C bus       
> directory: new_device and delete_device. Both files are write only and you      
> must write the right parameters to them in order to properly instantiate,       
> respectively delete, an I2C device.                                             
>                                                                                 
> File new_device takes 2 parameters: the name of the I2C device (a string)       
> and the address of the I2C device (a number, typically expressed in             
> hexadecimal starting with 0x, but can also be expressed in decimal.)            
>                                                                                 
> File delete_device takes a single parameter: the address of the I2C             
> device. As no two devices can live at the same address on a given I2C           
> segment, the address is sufficient to uniquely identify the device to be        
> deleted.                                                                        
>                                                                                 
> Example:                                                                        
> ```echo eeprom 0x50 > /sys/bus/i2c/devices/i2c-3/new_device```
>                                                                                 
> While this interface should only be used when in-kernel device declaration      
> can't be done, there is a variety of cases where it can be helpful:             
> * The I2C driver usually detects devices (method 3 above) but the bus           
>   segment your device lives on doesn't have the proper class bit set and        
>   thus detection doesn't trigger.                                               
> * The I2C driver usually detects devices, but your device lives at an           
>   unexpected address.                                                           
> * The I2C driver usually detects devices, but your device is not detected,      
>   either because the detection routine is too strict, or because your           
>   device is not officially supported yet but you know it is compatible.         
> * You are developing a driver on a test board, where you soldered the I2C       
>   device yourself.                                                              
>                                                                                 
> This interface is a replacement for the force_* module parameters some I2C      
> drivers implement. Being implemented in i2c-core rather than in each            
> device driver individually, it is much more efficient, and also has the         
> advantage that you do not have to reload the driver to change a setting.        
> You can also instantiate the device before the driver is loaded or even         
> available, and you don't need to know what driver the device needs.       


Taken from the [Linux kernel documentation](http://lxr.free-electrons.com/source/Documentation/i2c/instantiating-devices).

Considering this, we could instantiate a sensor (hih6130) connected to i2c-1 and with address `0x27` doing:

```
echo hih6130 0x27 > /sys/bus/i2c/devices/i2c-1/new_device 
```

After that, the device will be available under `/sys/bus/i2c/drivers/hih6130/1-0027`.

To remove the device you can use:
```
echo 0x27 > /sys/bus/i2c/devices/i2c-1/delete_device
```