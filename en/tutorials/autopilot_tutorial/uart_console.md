#UART and the console

A lot of components in ArduPilot rely on UARTs. They are used for debug output, telemetry, GPS modules and more. Understanding how to talk to the UARTs via the HAL will help you understand a lot of ArduPilot code.
The 5 UARTs

The ArduPilot HAL currently defines 5 UARTs. The HAL itself doesn’t define any particular roles for these UARTs, but the other parts of ArduPilot assume they will be assigned particular functions

    uartA – the console (usually USB, runs MAVLink telemetry)
    uartB – the first GPS
    uartC – primary telemetry (telem1 on Erle-Brain)
    uartD – secondary telemetry (telem2 on Erle-Brain)
    uartE – 2nd GPS

If you are writing your own sketch using the ArduPilot HAL then you can use these UARTs for any purpose you like, but if possible you should try to use the above assignments as it will allow you to fit in more easily to existing code.

Some UARTs have dual roles. For example there is a parameter SERIAL2_PROTOCOL changes uartD from being used for MAVLink versus being used for Frsky telemetry.

Go and have a look at the libraries/AP_HAL/examples/[UART_test](https://github.com/erlerobot/ardupilot/blob/master/libraries/AP_HAL/examples/UART_test/UART_test.pde) example sketch. It prints a hello message to all 5 UARTs. 

In order to build the example, follow the  next steps:

* Connect to Erle-Brain using `ssh` session.
* Place into `~/ardupilot/libraries/AP_HAL/examples/UART_test` folder.
* Compile the test typing `make pxf`.
* Move to the build folder `cd ~/build/UART_test.build`
* Execute the example `./UART_test`


Try it on your boards different ports. For example, you can use `screen` in order to see what UARTs are printing.

###Debug console

In addition to these 5 UARTs there is an additional debug console available. Try adding some ::printf() and other stdio functions to the UART_test sketch.


###UART Functions

Every UART has a number of basis IO functions available. The key functions are:

    printf – formatted print
    printf_P – formatted print with progmem string
    println – print and line feed
    write – write a bunch of bytes
    read – read some bytes
    available – check if any bytes are waiting
    txspace – check how much outgoing buffer space is available
    get_flow_control – check if the UART has flow control capabilities

Go and have a look at the declarations of each of these in AP_HAL and try them in UART_test.