---
title: Compile Batocera image
category: Development
order: 1
label: tbd
---

# Compilation Steps

Follow these steps to compile the firmware.

- Clone our repo:
  - HTTPS: ```git clone https://github.com/Ruka-CFW/batocera.linux.git```
  - SSH: ```git clone git@github.com:Ruka-CFW/batocera.linux.git```

- Change your directory to that folder and switch to our ```rk3128-wx8``` branch:
```
$ cd batocera.linux
$ git checkout rk3128-wx8
```
- Update submodules (buildroot) and switch to our ```rk3128-wx8``` branch:
```
git submodule update --init --recursive
git checkout rk3128-wx8
```

- Set the configuration for the ```PS5000``` with:
```
make O=$PWD/output/ps5000 BR2_EXTERNAL=$PWD -C $PWD/buildroot batocera-ps5000_defconfig
```
- Change directory to ```output/ps5000``` with: ```cd output/ps5000```
- Start compilation with ```make```

Depending on your system this may take more or less, but it will be about 1-4 hours

Once the build is complete, you should find the generated image in this path:
```output/ps5000/images/batocera/images/ps5000```

You will find the following files there:
- batocera-ps5000-32-20210923.img.gz  (this is the SDCard firmware)
- batocera-ps5000-32-20210923.img.gz.md5  
- batocera.version  
- boot.tar.xz  
- boot.tar.xz.md5

Flash the firmware with your preferred tool (e.g. Balena Etcher). You will need at least a 4GB card, but we recommend a minimum of 16GB. The system will automatically expand the card to use all the space available during the first boot.

