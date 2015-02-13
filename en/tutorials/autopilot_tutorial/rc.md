# RC Input and Output

RC input is a key part of any autopilot, giving the pilot control of the airframe, allowing them to change modes and also giving them control of auxiliary equipment such as camera mounts.

ArduPilot supports several different types of RC Input on Erle-Brain:

    PPMSum 
    SBUS 
    Spektrum/DSM 
    RC Override (MAVLink)

Note that SBUS and Spektrum/DSM are serial protocols. SBUS is a 100kbaud inverted UART protocol and Spektrum/DSM is a 115200 baud UART protocol. Erle-Brain implements them as bit-banged software UARTs.

RC Output is how ArduPilot controls servos and motors. The number of available output channels are 13, but the use dependens on the hardware you want to use. The quad-copter uses four channels to control the motors, hexa-copter six and octo-copter eight. But if you want to use and control a gimbal+camera, you will need another two output channels. The Erle-Plane uses four output channels, the Erle-Rover ony two. 

RC Output defaults to 50Hz PWM values, but can be configured for a wide range of update rates. For example, the ArduCopter code sets up its motor outputs to drive the ESCs at a much higher rate – typically over 400Hz.

###AP_HAL RCInput object

The first object to understand is the `AP_HAL` RCInput object which is available as `hal.rcin`. That provides low level access to the channel values currently being received on the board. The returned values are PWM values in microseconds.

Go and have a look at the [RCInput.pde](https://github.com/erlerobot/ardupilot/blob/master/libraries/AP_HAL/examples/RCInput/RCInput.pde) sketch and try it on your board. Try moving the sticks on your transmitter and check that the values change correctly in the output.

###AP_HAL RCOutput object

The AP_HAL RCOutput object (available as `hal.rcout`) gives low level control of all output channels. 

Go and have a look at the [RCOutput.pde](https://github.com/erlerobot/ardupilot/blob/master/libraries/AP_HAL/examples/RCOutput/RCOutput.pde) example sketch. You’ll see that it just sets up all the channels to wave the servos from minimum to maximum over a few second period. Hook up some servos to your board and then test to make sure it works for you.

###The RC_Channel object

The `hal.rcin` and `hal.rcout` objects discussed above are low level functions. The usual way of handling RC input and output in ArduPilot is via a higher level object called RC_Channel. That object has user configurable parameters for the min/max/trim of each channel, as well as support for auxillary channel function, scaling of inputs and outputs and many other features.

Go and have a look at [RC_Channel.pde](https://github.com/erlerobot/ardupilot/blob/master/libraries/RC_Channel/examples/RC_Channel/RC_Channel.pde). That example shows how to setup RC channels, read input and copy input to output values. Run that on your board and check that transmitter input is passed through to a servo. Try changing it to reverse a channel, and change the min/max/trim on a channel. Have a look through [RC_Channel.h](https://github.com/erlerobot/ardupilot/blob/master/libraries/RC_Channel/RC_Channel.h) to see what API functions are available.

###The strange input/output setup in RC_Channel

If you look at `RC_Channel` carefully you’ll notice something strange. A lot of variables apply to both the input side and the output side. For example, the `rc1->radio_trim` applies to channel 1 as an input (specifying the trim point for calculation a input proportion), plus as an output, specifying the midpoint of a servo attached to that channel.

This ties input channels numbers to output channel numbers, when really they are separate concepts. You may want to use channel 1 input as “roll input”, but use channel 1 output for steering a nose wheel on an aircraft. With `RC_Channel` you can’t really do that, or at least if you do it you will end up with some very odd code. This is an artifact of how `RC_Channel` was originally developed, where pass-thru from input channels to output channels on a fixed wing aircraft in manual mode was normal. Someday we may change it to break apart the two concepts in a more logical fashion, but for now just be aware that it is strange, so when you see odd bits of code working around this you’ll know why.

###The RC_Channel_aux object

Along with `RC_Channel` there is another important class in `libraries/RC_Channel`. It is the `RC_Channel_aux` class, which is a subclass of `RC_Channel`.

An `RC_Channel_aux` object is a type of `RC_Channel` but with additional properties that allow its purpose to be specified by a user. Say for example a user wants channel 6 to be a roll stabilization for a camera mount. They would set a parameter `RC6_FUNCTION` to be 21, which means “rudder”. Then another library could say:

	RC_Channel_aux::set_servo_out(RC_Channel_aux::k_rudder, 4500);

and any channel which has its FUNCTION set to 21 will move to full deflection (as k_rudder is setup scaled as an angle in centi-degrees with 4500 being the maximum). Note that this is a one to many arrangement. The user can setup multiple channels to have FUNCTION 21 if they want to, and all of those channels will move, each using their own min/max/trim values.