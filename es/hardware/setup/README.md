#Configuración del Hardware

En esta sección enseñaremos cómo debes hacer las conexiones de los conectores en cada dispositivo adicional.

*Nota*: ten cuidado cuando desconectes los dispositivos de los conectores DF13, la primera vez los conectores pueden estar rígidos.

En la próxima imágen, muestra el hardware más común adjuntado a Erle-Brain:

##Configuración típica del hardware

![Configuración Hardware](../../img/hardwaresetup/HardwareSetUp.png)



##PWM fuera
Erle-Brain tiene 13 canales de salida de PWM. En éste canal, los dispositivos más típicos que deberías conectar son: ESC, servos, servos gimbal,...

La siguiente imágen muestra cuales son los canales PWM:

![PWM setup](../../img/hardwaresetup/PWMsetup.png)

##Entrada RC
El receptor radio control debe estar conectado en el rail del canal 14, como se muestra abajo:

![RC setup](../../img/hardwaresetup/RCsetup.png)

##Zumbador
El zumbador debe estar conectado en los únicos dos pines del conector DF13 de Erle-Brain.

![Configuración Zumbador](../../img/hardwaresetup/Buzzersetup.png)

##GPS y Compás
Predeterminadamente, el GPS debe estar conectado al puerto *Serial*. Este puerto dará fuerza al dispositivo.
El bus *I2C* se usa para conectar el compás a Erle-Brain.

![Configuración GPS](../../img/hardwaresetup/GPSsetup.png)

##Bus I2C
Erle-Brain contiene tres conectores de buses *I2C*, que dan acceso al bus *I2C1*. En este bus puedes conectar múltiples tipos de dispositivos, ej: compás, sensores de gas, sensores de temperatura,...

![Configuración I2C](../../img/hardwaresetup/I2Csetup.png)

##Tensión
Usa éste conector para alimentar Erle-Brain desde el Power-Module.

![Configuración Tensión](../../img/hardwaresetup/PowerSetup.png)

##Otros buses/puertos

![Configuración Otros](../../img/hardwaresetup/OtherSetup.png)

##I/O
Este conector te permite conectar hardware externo a dos GPIOs. También proporciona con 3v3.

####CAN
Usa este conector para acceder al bus CAN de Erle-Brain

--------------------------------
*Nota*: Un buen ejemplo para usar los conectores *GPIO* y *CAN* son los [ubleds](https://www.youtube.com/watch?v=GHeZ_jrA8lg).

--------------------------------

####ADC
Erle-Brain también tiene un bus dedicado para corriente ADC, que contiene dos canales ADC. El concetor también proporciona 5 Voltios.


------
*Precaución*: El rango de entrada de los ADCs son 0~1.8v. No conectes ningún dispositivo que supere éste voltage.

------