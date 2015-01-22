# Quick start with Erle-Brain

### Connecting to Erle-Brain
There are several ways to connect to Erle-Brain:

##### Using a WiFi dongle
If you purchased your brain with WiFi you'll see that we attached and configured a dongle that will create automatically 

### Checking the processes
```bash
ps -e
```
You should see `ArduCopter.elf` and `apm4-startup.sh` running.

### Changing to other vehicle
Let's assume you wish to use Erle-Brain with plane:
```bash
cd ardupilot/ArduPlane
make configure
make pxf
# this will create a binary called ArduPlane.elf at ~/build
cp ~/ArduPlane.elf ~/
```

Now you'll need to update the startup scripts to launch plane instead of copter. To do so edit the `/etc/init.d/apm4-startup.sh` file and at the bottom of the file modify the ArduCopter launch call for an ArduPlane one.

### Launching ROS
```bash
roscore &
```

Now make check the nodes an topics available:
```bash
rosnode list
rostopic list
```