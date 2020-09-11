# embedded-common
This repository contains the Sensirion i2c hardware abstraction layer and
commonly shared code. It is a dependency of the Sensirion embedded drivers.
**It should not be necessary to clone this repository independently**, and we
recommend to clone the repository recursively from the respective projects.

If you're looking for sensor drivers, please refer to the release bundles (zip
files) from the respective projects. An overview is listed at
https://github.com/Sensirion/info#repositories

## Repository content
* `*.c` files (CRC calculation function)
* `*.h` files (type definitions, prototype declarations for i2c
  communication,...)
* `hw_i2c` folder (stub for implementation of the hardware i2c communication,
  including sample code for some platforms)
* `sw_i2c` folder (stub for implementation of the software i2c communication
  (bit-banging GPIOs), including sample code for some platforms)

### sensirion\_arch\_config.h
You need to specify the integer sizes on your system.
We assume you have the `<stdint.h>` file available, so we have an
include directive for it. If this is not the case on your platform you need to
specify the following types yourself:

* `int64_t` = signed 64bit integer
* `uint64_t` = unsigned 64bit integer
* `int32_t` = signed 32bit integer
* `uint32_t` = unsigned 32bit integer
* `int16_t` = signed 16bit integer
* `uint16_t` = unsigned 16bit integer
* `int8_t` = signed 8bit integer
* `uint8_t` = unsigned 8bit integer

Make sure you specify the whole range (64bit sized integers are used on a 32bit
system for some sensor data).

### hw\_i2c\_implementation.c
Use this file when using a dedicated i2c controller (use software i2c from the
next section for bit banging on GPIOs.)

Use this file to implement the i2c communication via the i2c controller of your
system. Implement the documented function stubs: an init, a read, a write and a
delay function. For further information please check out some of the sample
implementations in the `hw_i2c/sample-implementations` folder in this
repository.

### sw\_i2c\_implementation.c
Use this file if you don't have a dedicated hardware i2c controller (otherwise
see the previous section) to emulate an i2c controller in software by
bit-banging GPIOs. You will thus need to implement the stubs necessary to drive
the GPIO pins used for SDA (data line) and SCL (clock line) along with the delay
function, that must be precise enough to drive the pins at the desired
frequency.  For further information please check out some of the sample
implementations in the `sw_i2c/sample-implementations` folder in this
repository.
