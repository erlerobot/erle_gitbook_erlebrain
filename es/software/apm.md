# APM: El Piloto Multiplataforma

El proyecto Ardupilot comenzó oficialmente en 2009 con las contribuciones de Jordi Muñoz y Chris Anderson. El proyecto fue inicialmente vinculado con una plataforma hardware, manteniendo el mismo nombre que el software y fue usado en el desarrollo del entorno Arduino.

Como muchos desarrolladores se unieron al proyecto y aparecieron nuevas plataformas hardware, la necesidad de crear una capa de abstracción que permitiera diferentes placas soportadas por el código era obvia. En Agosto de 2012, Pat Hickey creó la [AP_HAL](https://github.com/erlerobot/ardupilot/tree/master/libraries/AP_HAL) capa de abstracción que simplificó el añadir soporte a las placas nuevas. Utilizando ésta idea, cada placa creará su propia Capa de Abstracción de Hardware (HAL), heredando los requisitos generales especificados en `AP_HAL`.

El proyecto creció lejos del editor Arduino, entonces se volvió a llamar APM, que reposa para la Multiplataforma Autopiloto. Durante el último cuarto de 2013, Andrew Tridgell hizo un prototipo de [AP_HAL_Linux](https://github.com/erlerobot/ardupilot/tree/master/libraries/AP_HAL_Linux), una capa de abstracción que permitía correr el código de APM en Linux usando la BeagleBone Black, como el proyecto original de hardware. Continuando este trabajo, los autores decidieron lanzar el proyecto BeaglePilot para continuar con el esfuerzo de hacer disponible el APM en los sistemas Linux.

Ahora el proyecto Ardupilot tiene unas 700.000 lineas de código, más de 150 contribuyentes, 8.4 commits de media al día (un commit cada 3 horas) y miles de usuarios en el mundo.

### Aprendiendo el código de APM
Los desarrolladores de APM están poniendo en conjunto un gran conjunto de tutoriales [here](http://dev.ardupilot.com/wiki/learning-the-ardupilot-codebase/) que ayudan a entender el código.

### Fuentes:
- [Wiki](http://ardupilot.com/)
- [Developers Wiki](http://dev.ardupilot.com/)
- [Learning APM code](http://dev.ardupilot.com/wiki/learning-the-ardupilot-codebase/)
