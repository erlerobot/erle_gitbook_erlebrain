# APM: Multiplatform Autopilot

Ardupilot project started officially in 2009 with the contributions of Jordi Muñoz and Chris Anderson . The project was initially linked to a hardware platform holding the same name as the software and it used the Arduino development environment.

As more developers joined the project and new hardware platforms appeared it seemed obvious the need of creating an abstaction layer that allowed different boards to be supported
by the code. In August of 2012, Pat Hickey created the [AP_HAL](https://github.com/erlerobot/ardupilot/tree/master/libraries/AP_HAL) abstraction layer that simplified adding support for new boards. Using this idea, each board would create its own Hardware Abstraction Layer (HAL) inheriting the general requirements specified in the `AP_HAL`.

The project grew far from the Arduino editor thereby it was renamed to APM which stands for AutoPilot Multiplatform. During the last quarter of 2013, Andrew Tridgell made a prototype of [AP_HAL_Linux](https://github.com/erlerobot/ardupilot/tree/master/libraries/AP_HAL_Linux), an abstraction layer that allowed to run APM’s code in Linux using the BeagleBone Black as the hardware blueprint. Following this work, the authors decided to launch BeaglePilot project to continue with the effort of making APM available for Linux systems. 

Currently ardupilot project has about 700.000 lines of code, more than 150 contributors, 8.4 commits per day on average (a commit every three hours) and thousands of users worldwide.

### Learning the APM code
APM developers are putting together a great set of tutorials available [here](http://dev.ardupilot.com/wiki/learning-the-ardupilot-codebase/) that helps understand the code.

### Sources:
- [Wiki](http://ardupilot.com/)
- [Developers Wiki](http://dev.ardupilot.com/)
- [Learning APM code](http://dev.ardupilot.com/wiki/learning-the-ardupilot-codebase/)