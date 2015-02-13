#Storage and EEPROM management

Content
* [The AP_HAL::Storage library](#the-ap_hal::storage-library)
* [The DataFlash library](#the-dataflash-library)
* [Posix IO](#posix-io)

Every board that ArduPilot supports has some form of persistent storage available. This is used to hold user parameters, waypoints, rally points, terrain data and many other useful things. To provide access to this storage ArduPilot uses in Erle-Brain has 3 basic mechanisms:

    the AP_HAL::Storage object, accessed as hal.storage
    the StorageManager library to give a higher level abstraction layer on hal.storage
    DataFlash for storing to an on-board logging area

The other libraries and functions that need persistent storage are built on these basic systems. For example, the AP_Param` library (which handles user settable parameters) is built on top of StorageManager, which in turn is built on top of `AP_HAL::Storage`. 

###The AP_HAL::Storage library

The `AP_HAL::Storage` object is available on all platforms. The minimum size of storage available via this interface on boards that ArduPilot supports is 4096 bytes.

The hal.storage API is extremely simple. It has just 3 functions

    init() to start up the storage subsystem
    read_block() to read a block of bytes
    write_block() to write a block of bytes

The reason for this very simple API is that developers are encouraged to use the StorageManager API instead of `hal.storage`. You should only be delving into `hal.storage` when doing bringup of a new board, or when debugging.

The size of storage available is set inside `AP_HAL_Boards.h` in the macro [HAL_STORAGE_SIZE](https://github.com/erlerobot/ardupilot/blob/master/libraries/AP_HAL/AP_HAL_Boards.h#L182). This means we don’t yet support dynamically determining the size of storage available for this interface. 

We don’t have an example sketch for the `AP_HAL::Storage` API, so this is your chance to write one. If you have gotten this far into the ArduPilot tutorial you should have seen enough example sketches to know how to write one from scratch. So write a `libraries/AP_HAL/examples/Storage` example that calculates an 8 bit XOR of the full contents of the `hal.storage` data, and prints it to the console. Then submit the example as a patch to the ArduPilot github, being careful to follow the patch submission guidelines.


###The StorageManager library

The simple hal.storage API makes it nice and easy to port ArduPilot to a new board, but isn’t convenient for actually using the available storage. For that we have StorageManager. The StorageManager library provides an API which abstracts the storage into pseudo-contiguous blocks of data that have an assigned purpose. So we chop up the available storage into areas that provide:

    parameters
    fence points
    waypoints
    rally points

Go and have a read of [StorageManager.cpp](https://github.com/erlerobot/ardupilot/blob/master/libraries/StorageManager/StorageManager.cpp). In particular look at the tables at the top. Notice how multiple areas of each type are defined for systems with have larger amounts of storage. This ability to combine multiple non-contiguous areas of storage into a single logical storage area was the main motivation for adding StorageManager. It allowed us to grow from using 4k of storage on all boards to using the full storage available on each board without asking users to reload all their parameters or reload their waypoints.

The StorageManager API also provides convenient functions for reading and writing variables like integers. This is the API that libraries like `AP_Mission` use to save and restore waypoints.

Now go and have a look at [StorageTest.pde](https://github.com/erlerobot/ardupilot/tree/master/libraries/StorageManager/examples/StorageTest). This is a stress test for the StoageManager layer, and as a result is also a stress test for the `AP_HAL::Storage` object. It does random IO at random offsets of random lengths. That means it does IOs that cross boundaries where a single parameter value may be non-contiguous in FRAM or EEPROM. This test sketch was designed to stress test the StorageManager API.

Go and try StorageTest on your board, but be warned that it is a destructive test. It won’t destroy your board, but it will wipe all your parameters and waypoints. So backup your configuration if you are testing on the board in your favourite aircraft.

As an exercise, add some profiling to StorageTest so it prints the total IO rate in kbytes/sec along with the IO rate for reads and writes. Do you notice something about the difference between the read and write rates? Do you notice anything about the speed of writing values which are already set at that address in storage? See if you can find the code in StorageManager that explains your observations. Then submit a patch adding the profiling output to the ArduPilot github.

Next go and search through the ArduPilot libraries to fine all the places that use the StorageManager. What you are looking for is StorageAccess handles, like this:

    StorageAccess AP_Param::_storage(StorageManager::StorageParam);

that declares a variable called `AP_Param::_storage` which provides access to the StorageParam area of the storage on this board. What libraries use StorageAccess?

###The DataFlash library

Another type of storage that autopilots need is for on-board logs. For ArduPilot that is provided by the DataFlash library. It is quite an odd library in many ways. For a start the name is weird and comes from the fact that it was originally designed around the DataFlash chip for the APM1. It was a hardware device driver library which morphed over time into a general logging system. Internally the structure of the library shows this history in several ways (and not good ways!).

These days the DataFlash API is designed around a logging infrastructure model. It allows you to define self-describing data structures for individual log messages – such as a “GPS” message to log data from a GPS sensor. It handles logging that data to persistent storage in an efficient manner and also provides APIs for other libraries to use to get the data back out when the user wants to download their log files after a flight.

If you have seen the *.bin` files that ArduPilot uses these days when you download a log then you have seen the format that ArduPilot uses to store log messages. It is “self describing”, meaning that the ground station can work out the format of the messages in the log file without having to have some common scheme. At the front of each log file is a set of FMT messages which have a well known format and which describe the format of the messages that follow.

Go and have a look at [DataFlash_test.pde](https://github.com/erlerobot/ardupilot/blob/master/libraries/DataFlash/examples/DataFlash_test/DataFlash_test.pde). You’ll see a little table at the top that defines the log messages we will be writing, in this case a ‘TEST’ message which contains 4 unsigned 16 bit integers and two signed 32 bit integers (that is what “HHHHii” means). It also gives names for those 6 variables (cunningly labelled V1 to V4 and L1 and L2).

In the `loop()` function you will see a rather strange call like this:

 DataFlash.get_log_boundaries(log_num, start, end);

This is the public API for the way that the DataFlash library hides how the board actually stores log files. On Erle-Brain, as has Posix IO, log files are stored as separate files in a “LOGS” directory on the microSD card. These files can be directly copied by a user by pulling out the microSD card and putting it into their PC.

###Posix IO

Linux ports have a very good Posix subsystems. You can take advantage of this in libraries for ArduPilot as long as you don’t rely on it for anything that has to work on all boards.

A good example of this is the AP_Terrain library, which holds terrain data. Terrain data is much too large to fit in EEPROM, and it is random access, so it isn’t suitable for DataFlash. It is also not essential for the basic functionality of an autopilot, so it is a great candidate for implementing using Posix IO.

To know where on the filesystem you should store the data you add a data specific macro in `AP_HAL_Boards.h` which gives the directory path where that sort of data should be placed. For example, the macro `HAL_BOARD_TERRAIN_DIRECTORY` is used to define the directory where terrain data should go.

Once you have those two things you should just use normal Posix IO functions (ie. open, close, read, write etc), although there are some caveats:

    you must only call IO functions from either the IO timer or from your own low priority thread.
    Never call any IO functions from APIs directly accessible in your library. Not even a simple one like stat().
    try to be friendly to slow storage cards, do IO in reasonably sized chunks, and avoid seeks where possible

These rules really matter. A simple IO on a microSD card could take up to a second. That is enough time for your precious quadcopter to be flying upside down and heading for its final meeting with the ground. The average time for an IO microSD card is quite low (a few milliseconds), but just occasionally you will get a slow one when the microSD card decides it need to spend some quality time re-reading the SD card specification and calculating Pi. Don’t be tempted to sneak a little operation in just because it seems fast most of the time.

The one exception to this is initialization functions that you know for certain can only be called when the vehicle is starting up or is disarmed. A bit of delay at that time is fine.

Now go and have a read of [TerrainIO.cpp](https://github.com/erlerobot/ardupilot/blob/master/libraries/AP_Terrain/TerrainIO.cpp) and look at how it uses Posix IO. Notice the little state machine it uses to handle all the IO, which is all called from the `AP_Terrain::io_timer` function. See if you can spot any bugs, and report them if you can!

