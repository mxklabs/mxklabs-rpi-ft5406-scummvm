# mxklabs-rpi-ft5406-scummvm
If you have the Raspberry Pi's and the official 7" [touchscreen](https://www.raspberrypi.org/products/raspberry-pi-touch-display/) you may have found that the default driver is not compatible with [ScummVM](https://www.scummvm.org/) (and possibly also other [SDL](https://www.libsdl.org/) applications). This is because the default Raspbian driver presents itself as a touchscreen device and touchscreen events are not picked up by [ScummVM](https://www.scummvm.org/). 

This repository presents a workaround in the form of an adapted the Raspberry Pi's touchscreen driver that presents itself as a mouse instead of a touchscreen, making it '[ScummVM](https://www.scummvm.org/)-compatible'. The driver also emits a 'F5' key press when you press the screen with two fingers, making it possible to save and load games without plugging in a keyboard.

## Quickstart
* This driver is compatible with [Rasberry Pi kernel](https://github.com/raspberrypi/linux) version `4.14.76-v7+` (tag `rpi-4.14.y`) and has been tested with [Raspbian GNU/Linux 9 (stretch)](https://www.raspberrypi.org/downloads/). I was not able to get this implementation to work with some other kernel versions but your mileage may vary.
* You can compile the driver out-of-tree simply by calling `make`, assuming you are running the `4.14.76-v7+` kernel and have the appropriate kernel headers installed (see below for more info).
* Loading the driver with `insmod` or `modprobe` after Raspbian has started up does not work (the driver loads but is unable to detect touchscreen events). You need to load the driver on startup by replacing `/lib/modules/4.14.76-v7+/kernel/drivers/input/touchscreen/rpi-ft5406.ko`, the default touchscreen driver.
* Note that using this driver is a quick workaround to the problem and not a proper fix. A proper fix would fix ScummVM so it would listen to touchscreen events.
