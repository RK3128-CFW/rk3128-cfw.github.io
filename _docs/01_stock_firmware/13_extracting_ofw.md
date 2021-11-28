---
title: Extracting the original firmware
category: Stock Firmware (OFW)
order: 3
label: wip
---

## Extracting U-Boot DTS

First, use `binwalk` to get the address of the DTB within the uboot image:
```shell
> binwalk -e uboot.img

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
260000        0x3F7A0         device tree image (dtb)
266428        0x410BC         SHA256 hash constants, little endian
297384        0x489A8         CRC32 polynomial table, little endian
331507        0x50EF3         Android bootimg, kernel size: 1952736768 bytes, kernel addr: 0x6464615F, ramdisk size: 7495538 bytes, ramdisk addr: 0x46206F4E, product name: "%s: failed to read dtb file, ret=%d"
488904        0x775C8         device tree image (dtb)
784288        0xBF7A0         device tree image (dtb)
790716        0xC10BC         SHA256 hash constants, little endian
821672        0xC89A8         CRC32 polynomial table, little endian
855795        0xD0EF3         Android bootimg, kernel size: 1952736768 bytes, kernel addr: 0x6464615F, ramdisk size: 7495538 bytes, ramdisk addr: 0x46206F4E, product name: "%s: failed to read dtb file, ret=%d"
1013192       0xF75C8         device tree image (dtb)
```

The last line contains the information we need (`1013192`). Use `dd` command
to extract the DTB file:
```shell
❯ dd if=uboot.img of=uboot.dtb bs=1 skip=1013192
35384+0 records in
35384+0 records out
35384 bytes (35 kB, 35 KiB) copied, 0,0946254 s, 374 kB/s
```

Finally, use `dtc` command to decompile the device tree binary:
```shell
❯ dtc -I dtb uboot.dtb -O dts -o uboot.dts
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
uboot.dts: Warning (unique_unit_address): /mipi-dphy@20038000: duplicate unit-address (also used in node /lvds@20038000)
uboot.dts: Warning (unique_unit_address): /syscon@20008000: duplicate unit-address (also used in node /pinctrl@20008000)
uboot.dts: Warning (graph_child_address): /lvds@20038000/ports: graph node has single child node 'port@0', #address-cells/#size-cells are not necessary
uboot.dts: Warning (graph_endpoint): /vop@1010e000/port/endpoint@2: graph node unit address error, expected "1"
```
