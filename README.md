# 4B25: 'A low-cost head-of-bed inclinometer'

#### Steve Mead, Darwin College, sfm36

## Project Summary

The purpose of the project is to develop a digital device that can measure the angle of inclination of a plane relative to the plane of flat level ground. The device is intended to be attached to the back of the head of a hospital bed so that it measures the head of bed (HOB) angle and therefore the inclination of the patient's torso as it rests against the HOB.

The device outputs a continuous signal that is proportional to the HOB angle for inpatients with abnormal intercranial pressure (ICP). The angle of the patient's torso can provide important context to the measurement of ICP but, at the moment, standard practice is to record this manually, which places a burden on nursing staff. The inclinometer signal can be coupled into ICP recording equipment and so automates the task carried out by staff.

The central part of the device hardware is a [Freedom KL03Z development board](https://www.nxp.com/design/development-boards/freedom-development-boards/mcu-boards/freedom-development-platform-for-kinetis-kl03-mcus:FRDM-KL03Z). This features an on board accelerometer part MMA8451Q, which is used for sensing the projection of the gravity vector on to the accelerometer axes. The measured co-ordinates of this projection are used to compute tilt angle. (Analog Devices, 2010)

There are two other peripherals: 
1. A 12-bit DAC part MCP4725 that provides the device voltage output with a gain of 25mV per degree of tilt.
2. A 96x64 RGB OLED driven by a SSD1331 chip that displays the angle of HOB inclination.

## Software Tools and Repository Layout

The firmware is a fork of Warp-Firmware, which is located at https://github.com/physical-computation/Warp-firmware. The target microcontroller is the Kinetis L Series KL03Z32VFK4 on the development board.

A number of software tools are required to create the KL03Z binary file. First, clone a forked repository of Warp-Firmware to a local p.c. running Ubuntu 16.04. Second, install the [ARM cross compiler](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) and [CMake](https://cmake.org/download/) on this p.c. using the method described in Appendix C of the [4B25 course handbook](http://physcomp.eng.cam.ac.uk/4B25-course-notes-and-cover-trimmed-v0.3/4B25-course-notes-and-cover-trimmed-with-embedded-fonts.html). Third, install [J-Link Software for Linux](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack) on the local p.c.

J-Link is used to load the compiled binaries to the KL03Z board. A debug adapter (OpenSDA) is built into the KL03Z board. The J-Link openSDA image file should be written to the KL03Z board using the instructions [here](https://www.segger.com/products/debug-probes/j-link/models/other-j-links/opensda-sda-v2/).

On the local p.c., navigate to the Warp-firmware folder `Warp-firmware/src/boot/ksdk1.1.0` and overwrite the existing files with the following files supplied by sfm36:
```
CMakeLists.txt
warp-kl03-ksdk1.1-boot.c
devMCP4725.c
devMCP4725.h
devMMA8451Q.c
devMMA8451Q.h
devSSD1331.c
devSSD1331.h
gpio_pins.c
gpio_pins.h
```
Navigate to the Warp-firmware folder `Warp-firmware/build/ksdk1.1` and overwrite `build.sh` with the file supplied by sfm36:
```
build.sh
```
Run build.sh from the command line. This compiles the files and libraries to a `Warp.srec` binary, which is created in the folder `Warp-firmware/build/ksdk1.1/work/demos/Warp/armgcc/Warp/release`. Run the following command to flash the binary to the KL03Z board, which must be connected by USB cable to the p.c.
```
JLinkExe -device MKL03Z32XXX4 -if SWD -speed 4000 -CommanderScript ../../tools/scripts/jlink.commands
```
Open a Terminal window in Ubuntu and enter the following command
```
JLinkRTTClient
```
The terminal will print the reading number and measurement of angle of inclination in a time series, for example as shown below:
```
Booting Warp, in 3... 2... 1...
 0, 4.7 degrees
 1, 1.8 degrees
 2, 0.9 degrees
 3, 0.3 degrees
 4, 0.1 degrees
 5, 0.1 degrees
 6, 0.1 degrees
 etc.
```
## References

(Analog Devices, 2010). 'Using an Accelerometer for Inclination Sensing Application Note'. AN-1057
https://www.analog.com/media/en/technical-documentation/application-notes/AN-1057.pdf (Accessed 2 December 2019)
