# Installing Linux on X96 Mini

## Prerequisites
Instructions are covering steps on a Windows machine. Should be similar for Linux.
Based upon (messy) instructions found at https://forum.armbian.com/topic/12162-single-armbian-image-for-rk-aml-aw-aarch64-armv8/

### Hardware
* X96 Mini. CPU is S905X.
* SD card, 16 GB
* Toothpick or small stick (reset button)

### Software
* Rufus 3.12
* 7Z

## Preparation
* Download https://users.armbian.com/balbes150/arm-64/Armbian_20.10_Arm-64_focal_current_5.9.0.img.xz
* Unzip this file to get Armbian_20.10_Arm-64_focal_current_5.9.0.img
* Launch Rufus, select the img. Click on 'Start'. SD card will be formatted and content will be written.

## Configuration
* In Windows Explorer, navigate to your SD card. You should see a structure a 'extlinux' folder, 'dtb', etc. 
* Edit the file /extlinux/extlinux.conf
* Comment out all lines starting with FDT and APPEND (we don't want RK or AW configuration, we are only interested in AML s9xxx section). 
* Uncomment 'FDT /dtb/amlogic/meson-gxl-s905x-p212.dtb' and 'APPEND ....'. See example below


```javascript
LABEL Armbian
LINUX /zImage
INITRD /uInitrd

# rk-3399
#FDT /dtb/rockchip/rk3399-rock-pi-4.dtb
#FDT /dtb/rockchip/rk3399-nanopc-t4.dtb
#FDT /dtb/rockchip/rk3399-roc-pc-mezzanine.dtb
#APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=uart8250,mmio32,0xff1a0000 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0

# rk-3328
#FDT /dtb/rockchip/rk3328-roc-pc.dtb
#FDT /dtb/rockchip/rk3328-box-trn9.dtb
#FDT /dtb/rockchip/rk3328-box.dtb
#APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=uart8250,mmio32,0xff130000 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0

# aw h6
#FDT /dtb/allwinner/sun50i-h6-tanix-tx6.dtb
#APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyS0,115200 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0 video=HDMI-A-1:e
#APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyS0,115200 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0 mem=2048M video=HDMI-A-1:e

# aml s9xxx
#FDT /dtb/amlogic/meson-gxbb-p200.dtb
FDT /dtb/amlogic/meson-gxl-s905x-p212.dtb
#FDT /dtb/amlogic/meson-gxm-q200.dtb
#FDT /dtb/amlogic/meson-g12a-x96-max.dtb
#FDT /dtb/amlogic/meson-g12b-odroid-n2.dtb
APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyAML0,115200n8 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0
```
