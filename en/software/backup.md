# Back-up Erle-Brain

If you are going to make changes in your Erle-Brain, we suggest you go backing up it.

### Backing Up remotely, creating an image.

Connect your computer with the Erle-Brain, this proccess can take a long, so if it is posible, connect them using a mini-USB cable.

Open a terminal in your computer, and go to the directory where you are going to save the status of your drone software. Then execute the following command:

````bash
nc -l 19000|bzip2 -d|dd bs=16M of=BBB.img
```

Open a new terminal, and access to Erle Brain using ssh (follow 2.1 section instructions if you have difficulties). Execute the next command in Erle-Brain.

```bash
dd bs=16M if=/dev/mmcblk0|bzip2 -c|nc 192.168.7.1 19000
```

*Please note that if you've connected by wireless, you'll have to change the IP*

You'll notice USR0 (the LED closest to the S1 button in the corner) will start to blink steadily, rather than the double-pulse "heartbeat" pattern that is typical when your BeagleBone Black is running the typical Linux kernel configuration.

The proccess will end when USR0 stops blinking like before. To make sure that has finished, you can open a new terminal, and execute the following command twice with 30 seconds of range between them.

````bash
du BBB.img
```

If the output is the same, then the procces has finished.
