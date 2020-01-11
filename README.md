#4B25:  'A low-cost head-of-bed inclinometer'

###Steve Mead, Darwin College, sfm36

##Project Summary
Some text here

##Repository layout

This firmware is a fork of Warp-Firmware, which is located at https://github.com/physical-computation/Warp-firmware. The target hardware is [Freedom KL03Z evaluation board](https://www.nxp.com/design/development-boards/freedom-development-boards/mcu-boards/freedom-development-platform-for-kinetis-kl03-mcus:FRDM-KL03Z)

To create the firmware binary, first clone Warp-Firmware to your GitHub repository and then clone the forked repository to a local desktop p.c. running Ubuntu 16.04. The [ARM cross compiler](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) and [CMake](https://cmake.org/download/) should be installed on this p.c. using the method described in Appendix C of the [4B25 course handbook](http://physcomp.eng.cam.ac.uk/4B25-course-notes-and-cover-trimmed-v0.3/4B25-course-notes-and-cover-trimmed-with-embedded-fonts.html). Finally, install [J-Link Software for Linux](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack) on the local p.c. This is used to load the compiled binaries to the KL03Z board.

DEB installer, 64-bitSEGGER JLINK tools which we will use to load the compiled
binaries to the FRDM board


Ubuntu, you can install the ARM cross compiler using
sudo apt-get install gcc-arm-none-eabi .
On macOS, you can get a version of the ARM cross-compiler tools that are
known to work, from here. You will also need to install cmake if you are using
a system where cmake is not already installed
