# 4B25: 'A low-cost head-of-bed inclinometer'

#### Steve Mead, Darwin College, sfm36

## Project Summary
The purpose of the project is to develop a digital device that can measure the angle of inclination of a plane relative the plane of the flat ground. The application is provide data on the angle of the torso of a patient in a hospital bed. Theto use attach the device to the back of the head of a hospital bed. The 

## Software Tools and Repository layout

The firmware is a fork of Warp-Firmware, which is located at https://github.com/physical-computation/Warp-firmware. The target hardware is the [Freedom KL03Z evaluation board](https://www.nxp.com/design/development-boards/freedom-development-boards/mcu-boards/freedom-development-platform-for-kinetis-kl03-mcus:FRDM-KL03Z).

A number of software tools are required to create the KL03Z binary file. First, clone Warp-Firmware to your GitHub repository and then clone the forked repository to a local p.c. running Ubuntu 16.04. Second, install the [ARM cross compiler](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) and [CMake](https://cmake.org/download/) on this p.c. using the method described in Appendix C of the [4B25 course handbook](http://physcomp.eng.cam.ac.uk/4B25-course-notes-and-cover-trimmed-v0.3/4B25-course-notes-and-cover-trimmed-with-embedded-fonts.html). Finally, install [J-Link Software for Linux](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack) on the local p.c. J-Link is used to load the compiled binaries to the KL03Z board. A debug adapter (OpenSDA) is built into the KL03Z board. The J-Link openSDA image file should be written to the KL03Z board using the instructions [here](https://www.segger.com/products/debug-probes/j-link/models/other-j-links/opensda-sda-v2/).

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
Run build.sh from the command line. This compiles the SREC binary which is stored in `Warp-firmware/build/ksdk1.1/work/demos/Warp/armgcc/Warp/release`. From the same folder, run the following command to write the binary to a KL03Z board connected by USB cable to the p.c.
```
JLinkExe -device MKL03Z32XXX4 -if SWD -speed 4000 -CommanderScript ../../tools/scripts/jlink.commands
```
Open another Terminal window and enter the following command
```
JLinkRTTClient
```
The terminal will print the measurement number and measurement of angle of inclination in a series, for example as shown below:
```
Booting Warp, in 3... 2... 1...
 0, 4.7 degrees
 1, 1.8 degrees
 2, 0.9 degrees
 3, 0.3 degrees
 4, 0.1 degrees
 5, 0.1 degrees
 6, 0.1 degrees
```

