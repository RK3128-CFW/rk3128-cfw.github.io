---
title: Status/To-Do list
category: General
order: 1
label: WIP
---

# What works
* Most of Batocera 33 functionality is working for both PS5000 and Powkiddy A12/13.

# What is missing
* Splash screen does not work
* Retroarch:
 * GL video driver does not work very well, performance can be poor with some core/roms combinations. In those cases use the SDL2 driver instead.
   * Open the ``batocera.conf`` file and add ``global.retroarch.video_driver="sdl2"`` at the end of the file
 * The overlay/bezels only work with the GL driver, disabling the overlay/bezels provides additional performance gains.
 * HDMI does not currently work
 * Audio jack may work on some models but won't turn off the main speaker audio
