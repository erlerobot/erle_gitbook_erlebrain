#Expanding the File System Partition on the microSD

Generally storage devices needs some structure that allows the operating system to locate and create new files. This is done by using Partitions and File systems. A partition is a section of a storage device, which is formatted with a File System, onto which the operating system creates a directory structure.By expanding the File Systems in the microSD card we can utilize the rest of the place for storage purpose. Steps for Expanding the File Systems in External microSD card:

- Insert the microSD card in the robot, boot and locate your partition. `lsblk`and `df -h` help me figure it out:
```
$lsblk 
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
mmcblk0     179:0    0   7.5G  0 disk 
|-mmcblk0p1 179:1    0    48M  0 part /boot/uboot
`-mmcblk0p2 179:2    0   7.4G  0 part /
```
```
$df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/mmcblk0p2  7.2G  3.3G  3.6G  49% /
devtmpfs        122M  4.0K  122M   1% /dev
none             25M  200K   25M   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            122M     0  122M   0% /run/shm
/dev/mmcblk0p1   48M  9.4M   39M  20% /boot/uboot
```
- To manage a Disk partitions we have to use `fdisk command:
```
$fdisk /dev/mmcblk0
```

Then enter `p` it should the size of your SD card. Depending on the size of your SD card, values may change:

```
$fdisk /dev/mmcblk0

Command (m for help): p

Disk /dev/mmcblk0: 8010 MB, 8010072064 bytes
4 heads, 16 sectors/track, 244448 cylinders, total 15644672 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

        Device Boot      Start         End      Blocks   Id  System
/dev/mmcblk0p1   *        2048      100351       49152    e  W95 FAT16 (LBA)
/dev/mmcblk0p2          100352    15523839     7711744   83  Linux
```

 - Enter `d` command to delete a partition and mention the partition number as `2` .Entering `p` again will show that you have deleted /dev/mmcblk0p2

   - Now create a new partition by entering `n` then `p` and then `2`

   - n–>new partition

   - p–>primary (partition type)

   - 2–>partition number


- You can enter `p` again to see that the `/dev/mmcblk0p2` has been re-appeared.

- If everything goes fine enter `w` to commit your changes or if you are finding any error, `Ctrl+z` to stop the process.

- Reboot your system.
```
$ reboot
```

- After rebooting you need to expand the file system. Enter the command `df` (df–> displays the total space and free space available on your system)

- Then enter the following command to expand the file system. (It take some time to complete)
```
$ resize2fs /dev/mmcblk0p2      
```

After completing the process again type `df` to check the space available. Now you can access full space of your SD card. 

### Sources:
- [Expanding the File System Partition on a SD card in BeagleBone Black](http://blogspot.tenettech.com/?p=2932)