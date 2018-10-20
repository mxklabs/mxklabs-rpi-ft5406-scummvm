# mxklabs-rpi-ft5406-scummvm
If you have the Raspberry Pi's and the official 7" [touchscreen](https://www.raspberrypi.org/products/raspberry-pi-touch-display/) you may have found that the default driver is not compatible with [ScummVM](https://www.scummvm.org/) (and possibly also other [SDL](https://www.libsdl.org/) applications). This is because the default Raspbian driver presents itself as a touchscreen device and touchscreen events are not picked up by [ScummVM](https://www.scummvm.org/) for some reason. 

This repository presents a workaround in the form of a Raspberry Pi's touchscreen driver that presents itself as a mouse instead of a touchscreen, making it '[ScummVM](https://www.scummvm.org/)-compatible'. The driver also emits a 'F5' key press when you press the screen with two fingers, making it possible to save and load games without plugging in a keyboard.

To use this driver, follow either the quickstart or slowstart below.

* *Quickstart*: If you're lucky and have some knowledge of linux kernel development these quickstart steps may be all you need.
* *Slowstart*: A method that should work for everyone but unfortunately quite time consuming.

## Quickstart
* This driver is compatible with [Rasberry Pi kernel](https://github.com/raspberrypi/linux) version `4.14.76-v7+` (tag `rpi-4.14.y`) and has been tested with [Raspbian GNU/Linux 9 (stretch)](https://www.raspberrypi.org/downloads/). I was not able to get this implementation to work with some other kernel versions but your mileage may vary. See what kernel you are running by calling `uname -r`. If it is `4.14.76-v7+` you can continue with this quickstart and install the associated kernel headers with `sudo apt-get install raspberrypi-kernel-headers`. 
* Compile the driver out-of-tree simply by calling `make`. If this fails you are probably not running with a `4.14.76-v7+` kernel or you don't have the appropriate kernel headers installed and the quickstart won't work for you. Note that if you are running a different kernel the driver may still compile but the resulting kernel module may not work.
* Following compilation, test whether or not you can load the driver successfully by using `sudo insmod mxklabs-rpi-ft5406-scummvm.ko` and check for errors in the `dmesg` log. If you see any error traces related to the driver you're probably not running `4.14.76-v7+` or your kernel was compiled with a different compiler and this quickstart won't work for you. Note that a message to say your kernel is tainted is normal with this quickstart method. Also, if all is well don't worry if the touch screen does not work at this point.
* Loading the driver with `insmod` is a good way of testing whether the driver was compiled correctly but doesn't result in a functioning touchscreen. The next step is to load the driver on start-up by replacing the default touchscreen driver file`/lib/modules/4.14.76-v7+/kernel/drivers/input/touchscreen/rpi-ft5406.ko` with the `mxklabs-rpi-ft5406-scummvm.ko`  driver you compiled. Make sure the permissions and ownerships as per the original `rpi-ft5406.ko`. It is probably a good idea to make a backup of the original `rpi-ft5406.ko` so that you can switch back later if you want to. Now restart your Pi and you're done!

## Slowstart
TBD

## Credits
* The driver in this repository is mostly copied from the original `rpi-ft5406.c` file in the official Raspberry Pi [linux repository](https://github.com/raspberrypi/linux/). 

## Notes
* Note that this is a bit of a dirty, hacky way of fixing the problem. A proper fix would be changing ScummVM so it would listen to to touchscreen events (but that's less interesting). The implementation is also not really production-ready code in my opinion. I made it available anyway because it works and may be useful to someone else.
* Note that using this driver will mean you loose multi-touch capabilities. Revert back to the original `rpi-ft5406.ko` driver to get it back.
