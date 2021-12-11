---
title: Console Models & Revisions
category: General
order: 1
label: wip
---

| Console model | Rev | Display  | U-Boot DTS (Stock)    | Kernel DTS (Stock)  |
| ------------- | --- | -------- | --------------------- | ------------------- |
| Powkiddy A12  | A   | 1024x600 | [uboot_a12_rev_a.dts] | [boot_a12_rev_a.dts] |
| Powkiddy A12  | B   | 1024x600 | [uboot_a12_rev_b.dts] | [boot_a12_rev_b.dts] |
| Powkiddy A12  | C   | 800x480  | [uboot_a12_rev_c.dts] | [boot_a12_rev_c.dts] |
| Powkiddy A12  | D   | 800x480  | [uboot_a12_rev_d.dts] | [boot_a12_rev_d.dts] |
| Powkiddy A13  | A   | 1024x600 | [uboot_a13_rev_a.dts] | [boot_a13_rev_a.dts] |
| Powkiddy A13  | B   | 1024x600 | To be added           | To be added          |
| PS5000        | -   | 960x544  | [uboot_ps5000.dts]    | [boot_ps5000.dts]    |
| PS7000        | -   | 1024x600 | To be added           | To be added          |

[uboot_a12_rev_a.dts]: /files/dts/uboot_a12_rev_a.dts
[uboot_a12_rev_b.dts]: /files/dts/uboot_a12_rev_b.dts
[uboot_a12_rev_c.dts]: /files/dts/uboot_a12_rev_c.dts
[uboot_a12_rev_d.dts]: /files/dts/uboot_a12_rev_d.dts
[uboot_a13_rev_a.dts]: /files/dts/uboot_a13_rev_a.dts
[uboot_ps5000.dts]: /files/dts/uboot_ps5000.dts
[boot_a12_rev_a.dts]: /files/dts/boot_a12_rev_a.dts
[boot_a12_rev_b.dts]: /files/dts/boot_a12_rev_b.dts
[boot_a12_rev_c.dts]: /files/dts/boot_a12_rev_c.dts
[boot_a12_rev_d.dts]: /files/dts/boot_a12_rev_d.dts
[boot_a13_rev_a.dts]: /files/dts/boot_a13_rev_a.dts
[boot_ps5000.dts]: /files/dts/boot_ps5000.dts

## Differences among A12 Models

### A12 Rev A
Function keys have a different mapping. B, C & D share the same mapping:
```shell
*** boot_a12_rev_a.dts	2021-12-11 21:49:22.000000000 +0100
--- boot_a12_rev_b.dts	2021-12-10 22:04:03.000000000 +0100
***************
*** 702,709 ****

  		esckey {
  			debounce-interval = <0x14>;
! 			gpios = <0x34 0x08 0x01>;
! 			label = "vol up key";
  			linux,code = <0x3d>;
  		};

--- 702,709 ----

  		esckey {
  			debounce-interval = <0x14>;
! 			gpios = <0x5f 0x15 0x01>;
! 			label = "settings key";
  			linux,code = <0x3d>;
  		};

***************
*** 723,737 ****

  		voldownkey {
  			debounce-interval = <0x14>;
! 			gpios = <0x5f 0x15 0x01>;
! 			label = "settings key";
  			linux,code = <0x4a>;
  		};

  		volupkey {
  			debounce-interval = <0x14>;
! 			gpios = <0x34 0x0a 0x01>;
! 			label = "vol down key";
  			linux,code = <0x4e>;
  		};

--- 723,737 ----

  		voldownkey {
  			debounce-interval = <0x14>;
! 			gpios = <0x34 0x0a 0x01>;
! 			label = "vol down key";
  			linux,code = <0x4a>;
  		};

  		volupkey {
  			debounce-interval = <0x14>;
! 			gpios = <0x34 0x08 0x01>;
! 			label = "vol up key";
  			linux,code = <0x4e>;
  		};
```

### A12 Rev A & B
Display is 1024x600, `bat_table` is also different from C & D.
```shell
bat_table = <0x00 0x00 0x00 0x00 0xc8 0xc8 0xd98 0xe91 0xed3 0xf04 0xf22 0xf51 0xf89 0xfae 0xfbd 0xfcb 0x100d 0xd98 0xe91 0xed3 0xf04 0xf22 0xf51 0xf89 0xfae 0xfbd 0xfcb 0x100d>;

clock-frequency = <0x47b7600>;

hactive = <0x400>;
vactive = <0x258>;
```

### A12 Rev C
Display is 800x480, `bat_table` is also different from A & B.
```shell
bat_table = <0x00 0x00 0x00 0x00 0xc8 0xc8 0xdb6 0xdc9 0xdcf 0xe28 0xe64 0xe83 0xec1 0xefa 0xf2e 0xf3d 0xf6f 0xdb6 0xdc9 0xdcf 0xe28 0xe64 0xe83 0xec1 0xefa 0xf2e 0xf3d 0xf6f>;

clock-frequency = <0x3e2df80>;

hactive = <0x320>;
vactive = <0x1e0>;
```

### A12 Rev D
Same as **Rev C**, except `clock-frequency` (probably due to a new display):
```shell
clock-frequency = <0x2191c00>;
```
