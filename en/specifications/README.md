# Erle-Brain Specifications

### Electrical specifications

![](https://erlerobotics.com/blog/wp-content/uploads/2014/12/IMG_6334.jpg)

|  |  |
|---------------|------------------|
| **Power supply** | 5V supply|
|  | [Power module conector]() |
|| ESC signal cables (need dome soldering)|
|**Indicators**| 3 normal LEDs |
||RGB LED|
|**Connectors**| 4x UARTs |
||1 ADC conector|
||1 CAN |
||3x I2C|
||1 Buzzer out|
||1 Safety switch|
||12 PWM out channels|
||1 PPM/S.Bus out |
||1 PPM/S.Bus in |
||1 Spectrum in|
||1 Power brick in|
||1 Backup battery conector|

#### Power consumption

| MODE | consumption (mA) |
|------|------------------|
|Idling @ UBoot | 235 |
|Kernel Booting (Peak) | 442 |
|Kernel Idling | 323 |
|Dongle WiFi creating hotspot | 469 |
|Autopilot (APM) + ROS | 397 |
|Autopilot (APM) + ROS + Dongle in hotspot | 569 |

### Mechanical specifications

| **Size** | 95.6 x 75.27 x 36.2 mm |
|---------------|------------------|
|**Layers**| 6 (BBB) + 6 (PXF)|
|**PCB Thickness**| 1.6|
|**Weight**|110 grams|
