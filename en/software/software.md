# Software

The following sections will describe the software used within [Erle-Brain](http:/erlerobotics.com/blog/erle-brain). Briefly:

- Linux 3.8 kernel compiled with the PREEMPT option (best results we measured)
- Debian Wheezy file system
- ROS Indigo
- APM running natively in Linux (Copter image preinstalled)
- mavros ROS package (bridge between ROS and APM)
- preconfigured daemons for launching everything automatically, WiFi dongles support