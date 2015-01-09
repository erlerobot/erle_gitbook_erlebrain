#How to get serial over usb

### Linux or Mac OS

Recent OSs include FTDI driver by default so getting a serial connection is as easy as:

- Connect the miniUSB cable.
- If everything is installed, two new devices should appear under **/dev**: `/dev/ttyUSB0` and `/dev/ttyUSB1`.
- Launch a serial connection against `/dev/ttyUSB1` with:


```
screen /dev/ttyUSB1 115200
```

- If everything is right (assuming SD card is not connected) you should see some "C"s printed in the screen:

```
CCCCC
```

*(you can also use [minicom](http://en.wikipedia.org/wiki/Minicom) instead of screen).*


### Windows 7

#### Install the drivers
- [BONE_D64.exe (64 bits)](http://beagleboard.org/static/beaglebone/latest/Drivers/Windows/BONE_D64.exe) 
- [BONE_DRV.exe (32 bits)](http://beagleboard.org/static/beaglebone/latest/Drivers/Windows/BONE_DRV.exe)

#### Finding the Erle’s virtual serial port

- Start > Devices and Printers
- Look for BeagleBone/XDS 100v2
- Double-click or right-click it
- Click the Hardware tab.
- Under “Device Functions” is a list ports with the type of port in the next column.
- One of them is labeled “USB Serial Port (COMn)”. In my case it’s ”USB Serial Port (COM5)”

#### Obtaining and running a terminal program
For this purpose we're using [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/). PuTTY is a single `.EXE` file. There’s no installer. I created a directory called `C:\Program Files (x86)\putty`.

I downloaded putty.exe and copied into that directory. Then right-clicked putty.exe and chose “Pin to Start Menu” to make it easy.

#### Configure the terminal program to use SSH over the (virtual) serial port:
- Launch PuTTY. It’s a simple program and its main window is titled “PuTTY Configuration”
- Make sure Serial is checked. A serial port configuration dialog appears.
- Under “Serial line” enter the number of the COM port you discovered previously, COM5 in this case.
- Enter 115200 in the “Speed” field
- Click the Open button.
- Now reset the robot (pressing the reset button) and you should see:
   - (if **no proper** microSD card was introduced) "CCCC.."
   - (if a **proper** microSD card was introduced) the kernel booting.
