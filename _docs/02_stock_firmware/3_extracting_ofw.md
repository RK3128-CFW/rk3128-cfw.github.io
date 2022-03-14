---
title: Extracting the original firmware
category: Stock Firmware (OFW)
order: 3
label: wip
---

## Extracting U-Boot DTS

First, use `binwalk` to get the address of the DTB within the uboot image:
```shell
> binwalk uboot.img

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
261444        0x3FD44         SHA256 hash constants, little endian
292384        0x47620         CRC32 polynomial table, little endian
325267        0x4F693         Android bootimg, kernel size: 1952736768 bytes, kernel addr: 0x6464615F, ramdisk size: 7495538 bytes, ramdisk addr: 0x46206F4E, product name: "%s: failed to read dtb file, ret=%d"
480496        0x754F0         Flattened device tree, size: 20891 bytes, version: 17
785732        0xBFD44         SHA256 hash constants, little endian
816672        0xC7620         CRC32 polynomial table, little endian
849555        0xCF693         Android bootimg, kernel size: 1952736768 bytes, kernel addr: 0x6464615F, ramdisk size: 7495538 bytes, ramdisk addr: 0x46206F4E, product name: "%s: failed to read dtb file, ret=%d"
1004784       0xF54F0         Flattened device tree, size: 20891 bytes, version: 17
```

The last line contains the information we need (`1004784`). Use `dd` command
to extract the DTB file:
```shell
❯ dd if=uboot.img of=uboot.dtb bs=1 skip=1004784
43792+0 records in
43792+0 records out
43792 bytes transferred in 0.235705 secs (185792 bytes/sec)
```

Finally, use `dtc` command to decompile the device tree binary with `-s`option
(`Sort nodes and properties before outputting (useful for comparing trees`)::
```shell
❯ dtc -s -I dtb uboot.dtb -O dts -o uboot.dts
uboot.dts: Warning (unit_address_vs_reg): /memory: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/crypto: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/core: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/peri: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/gpu: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/vpu: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/rga: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/ebc: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/iep: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/lcdc: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /cpu_axi_bus/qos/vip: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_vs_reg): /usb2-phy: node has a reg or ranges property, but no unit name
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x000: unit name should not have leading "0x"
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x000: unit name should not have leading 0s
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x001: unit name should not have leading "0x"
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x001: unit name should not have leading 0s
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x002: unit name should not have leading "0x"
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x002: unit name should not have leading 0s
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x003: unit name should not have leading "0x"
uboot.dts: Warning (unit_address_format): /cpus/cpu@0x003: unit name should not have leading 0s
uboot.dts: Warning (spi_bus_bridge): /pinctrl@20008000/spi: incorrect #address-cells for SPI bus
uboot.dts: Warning (spi_bus_bridge): /pinctrl@20008000/spi: incorrect #size-cells for SPI bus
uboot.dts: Warning (spi_bus_reg): Failed prerequisite 'spi_bus_bridge'
uboot.dts: Warning (avoid_unnecessary_addr_size): /dsi@10110000: unnecessary #address-cells/#size-cells without "ranges" or child "reg" property
uboot.dts: Warning (unique_unit_address): /mipi-dphy@20038000: duplicate unit-address (also used in node /lvds@20038000)
uboot.dts: Warning (unique_unit_address): /syscon@20008000: duplicate unit-address (also used in node /pinctrl@20008000)
uboot.dts: Warning (interrupt_provider): /pinctrl@20008000/gpio0@2007c000: Missing #address-cells in interrupt provider
uboot.dts: Warning (interrupt_provider): /pinctrl@20008000/gpio1@20080000: Missing #address-cells in interrupt provider
uboot.dts: Warning (interrupt_provider): /pinctrl@20008000/gpio2@20084000: Missing #address-cells in interrupt provider
uboot.dts: Warning (interrupt_provider): /pinctrl@20008000/gpio2@20088000: Missing #address-cells in interrupt provider
uboot.dts: Warning (graph_endpoint): /vop@1010e000/port/endpoint@2: graph node unit address error, expected "1"
```

## Extracting Boot DTS

First, use `binwalk` to get the address of the DTB within the boot image:
```shell
> binwalk boot.img

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Android bootimg, kernel size: 3908048 bytes, kernel addr: 0x10008000, ramdisk size: 0 bytes, ramdisk addr: 0x11000000, product name: ""
2048          0x800           Linux kernel ARM boot executable zImage (little-endian)
27332         0x6AC4          gzip compressed data, maximum compression, from Unix, last modified: 1970-01-01 00:00:00 (null date)
3913728       0x3BB800        Flattened device tree, size: 48575 bytes, version: 17
3962368       0x3C7600        PC bitmap, Windows 3.x format,, 1024 x 600 x 24
5806080       0x589800        PC bitmap, Windows 3.x format,, 1024 x 600 x 24
```
The device tree starts at `3913728` and its size is `3962368 - 3913728 = 48640`.
Use `dd` command to extract the DTB file:
```shell
❯ dd if=boot.img of=boot.dtb bs=1 count=48640 skip=3913728
48640+0 records in
48640+0 records out
48640 bytes transferred in 0.521341 secs (93298 bytes/sec)
```

Finally, use `dtc` command to decompile the device tree binary with `-s`option
(`Sort nodes and properties before outputting (useful for comparing trees`):
```shell
❯ dtc -s -I dtb boot.dtb -O dts -o boot.dts
boot.dts: Warning (unit_address_vs_reg): /sound/simple-audio-card,dai-link@0: node has a unit name, but no reg or ranges property
boot.dts: Warning (unit_address_vs_reg): /sound/simple-audio-card,dai-link@1: node has a unit name, but no reg or ranges property
boot.dts: Warning (unit_address_format): /gpu@0x10091000: unit name should not have leading "0x"
boot.dts: Warning (unit_address_format): /reserved-memory/drm-logo@00000000: unit name should not have leading 0s
boot.dts: Warning (spi_bus_bridge): /pinctrl/spi: incorrect #address-cells for SPI bus
boot.dts: Warning (spi_bus_bridge): /pinctrl/spi: incorrect #size-cells for SPI bus
boot.dts: Warning (spi_bus_reg): Failed prerequisite 'spi_bus_bridge'
boot.dts: Warning (avoid_unnecessary_addr_size): /syscon@100a0000: unnecessary #address-cells/#size-cells without "ranges" or child "reg" property
boot.dts: Warning (avoid_unnecessary_addr_size): /dsi@10110000: unnecessary #address-cells/#size-cells without "ranges" or child "reg" property
boot.dts: Warning (avoid_unnecessary_addr_size): /hdmi@20034000: unnecessary #address-cells/#size-cells without "ranges" or child "reg" property
boot.dts: Warning (unique_unit_address): /cif@1010a000: duplicate unit-address (also used in node /cif-new@1010a000)
boot.dts: Warning (unique_unit_address): /mipi-dphy@20038000: duplicate unit-address (also used in node /lvds@20038000)
boot.dts: Warning (interrupt_provider): /pinctrl/gpio0@2007c000: Missing #address-cells in interrupt provider
boot.dts: Warning (interrupt_provider): /pinctrl/gpio1@20080000: Missing #address-cells in interrupt provider
boot.dts: Warning (interrupt_provider): /pinctrl/gpio2@20084000: Missing #address-cells in interrupt provider
boot.dts: Warning (interrupt_provider): /pinctrl/gpio3@20088000: Missing #address-cells in interrupt provider
boot.dts: Warning (graph_child_address): /hdmi@20034000/port: graph node has single child node 'endpoint@0', #address-cells/#size-cells are not necessary
```
