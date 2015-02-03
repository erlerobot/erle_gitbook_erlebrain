# Quick start with Erle-Brain

### Connecting to Erle-Brain
There are several ways to connect to Erle-Brain:

##### Using a WiFi dongle (5 GHz frequency)
If you purchased your brain with WiFi you'll see that we attached and configured a dongle that will create automatically a WiFi network (hotspot mode) with names with an `erle` preffix. The IP address of Erle-Brain is generally `11.0.0.1` and your machine should get the `11.0.0.2` (DHCP server has been configured to assign only one address).

----

**Make sure that your laptop/phone/tablet/... has 5 GHz support. You should look for 802.11 ac support.**

----

----

Erle-Brain creates a network called "erle-something" or "erle".
The password for the network is **holaerle**.

-----

##### Through mini USB
Erle-Brain supports client mode USB. Using this connection mechanism and the Ethernet-over-USB kernel module you should be able to SSH into the board.

To do so, connect Erle-Brain using the mini USB connector:

![](https://erlerobotics.com/blog/wp-content/uploads/2014/12/IMG_6334.jpg)

Find the new network interface that should've been created in your OS and assign the following IP address: `192.168.7.1`. Assuming that your new interface is eth6 and you are in Linux: 
```bash
sudo ifconfig eth6 192.168.7.1
```

Now that you are in the same subnet just ssh into the board:
```bash
ssh root@192.168.7.2
```

##### Using a GCS (MissionPlanner, APMPlanner, DroidPlanner, etc)
Select `UDP` and use the IP address `11.0.0.1` and the port `6000`.

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