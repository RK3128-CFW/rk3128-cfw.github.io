---
title: Compile Batocera image
category: Development
order: 1
---

# Compilation Steps

Follow these steps to compile the firmware.

- Clone our repo:
  - HTTPS: ```git clone https://github.com/RK3128-CFW/batocera.linux.git```
  - SSH: ```git clone git@github.com:RK3128-CFW/batocera.linux.git```

- Change your directory to that folder and switch to our ```rk3128-cfw``` branch:
```
$ cd batocera.linux
$ git checkout rk3128-cfw
```
- Update submodules (buildroot) and switch to our ```rk3128-cfw``` branch:
```
git submodule update --init --recursive
git checkout rk3128-cfw
```

- Build the rk3128 images (PS5000 & Powkiddy A12/A13) with:
```
make rk3128-build
```

Depending on your system this may take more or less, but it will be about 1-4 hours

Once the build is complete, you should find the generated images in these path:
* ```output/rk3128/images/batocera/images/ps5000```
* ```output/rk3128/images/batocera/images/powkiddy_a13```

You will find the following files there:
- batocera-rk3128-ps5000-33-20220311.img.gz (this is the SDCard firmware)
- batocera-rk3128-ps5000-33-20220311.img.gz.md5  
- batocera.version  
- boot.tar.xz  
- boot.tar.xz.md5

Flash the firmware with your preferred tool (e.g. Balena Etcher). You will need at least a 4GB card, but we recommend a minimum of 16GB. The system will automatically expand the card to use all the space available during the first boot.

