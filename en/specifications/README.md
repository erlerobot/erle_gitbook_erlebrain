# Specifications

### Electrical specifications

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

|  |  |
|---------------|------------------|
| **Size** | 85 x 55 cm|
|**Layers**| 6 |
|**PCB Thickness**| 1.6|
|**RoHS Compliant**|yes|
