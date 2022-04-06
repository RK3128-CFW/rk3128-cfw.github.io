---
title: Installation
category: Getting Started
order: 1
---

The following instructions provide the basic steps to install or update the firmware on your device, but if you need further information please follow the rest of the installation instructions and details from the [batocera wiki](https://wiki.batocera.org/install_batocera).

## Installation from scratch

* Download the latest release of the firmware for your console from the [releases page](https://github.com/RK3128-CFW/rk3128-cfw.github.io/releases)
* Download [Balena Etcher](https://www.balena.io/etcher/) for your operative system
* Install and run Balena Etcher
* Open the firmware file and select your SD card as your target device. Note that you will need a minimum of 8GB but we recommend a minimum of 16GB.
* Click on Flash! and wait for the image to be created.
* Once the image is created, insert it on your console, and boot.
* Note that the first time the system boot it takes a bit longer because Batocera automatically extends the image to use all the available space. After that first boot, the system should take about 30s to boot.

## Updating batocera without flashing

* Download the ``boot.tar.xz`` for the latest release from the [releases page](https://github.com/RK3128-CFW/rk3128-cfw.github.io/releases)
* Insert the SDCARD with your existing batocera installation on your PC/Mac/Linux
* Mount the first partition called BOOT
* Extract all the contents of the ``boot.tar.xz`` file and copy the contents to the BOOT partition
* Eject the SDCARD
* Insert the SDCARD on your console and boot
* Enjoy
* 
