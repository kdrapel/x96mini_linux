# Installing Linux on X96 Mini
![x96](/x96-mini-smart-tv-box-android-71-s905w.jpg)

Goal is to install a Linux on a X96 Mini that I did not use anymore as a TV box and wanted to recycle as a DNS server for ads-blocking (https://pi-hole.net/) and other experiments. 

## Prerequisites
Instructions are covering steps on a Windows machine. Should be similar for Linux.
Based upon (messy) instructions found at https://forum.armbian.com/topic/12162-single-armbian-image-for-rk-aml-aw-aarch64-armv8/

### Hardware
* X96 Mini 2GB CPU is S905X. Label behind say "X96 mini RAM 2GB, ROM 16 GB".
* SD card, 16 GB. Smaller will be ok too (needs at least 8GB)
* Toothpick or small stick (reset button *inside* AV jack)

### Software
* Rufus 3.12
* 7Z

## Preparation
* Download https://users.armbian.com/balbes150/arm-64/Armbian_20.10_Arm-64_focal_current_5.9.0.img.xz
* Unzip this file to get Armbian_20.10_Arm-64_focal_current_5.9.0.img
* Launch Rufus, select the img. Click on 'Start'. SD card will be formatted and content will be written.

## Configuration of u-boot
* In Windows Explorer, navigate to your SD card. You should see a structure a 'extlinux' folder, 'dtb', etc. 
* Rename the file 'u-boot-s905x-s912' to 'u-boot.ext'
  
## Configuration of device tree block
* A Device Tree Block (DTB) is a file that contains important information about the target hardware (more info http://junyelee.blogspot.com/2015/07/a-tutorial-on-device-tree.html). So it is necessary to use the proper one. This is a tricky part and if an improper DTB is used, your target system will fail loading or the kernel will panic.
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
## Launching
* Unplug the X96 Mini
* Insert the SD card
* Using a toothpick, small stick or whatever suitable, press on the 'reset' switch which is located *inside* the AV jack. You don't need to press too hard.
* While the reset switch is maintained pressed, plug the power. The X96 Mini screen will appear and normally after a few seconds, it should switch to the Linux boot. 
* You can release the reset switch. Linux should run if everything is properly configured.
* I access it through SSH on port 22. It is of course recommended to change the default root password (root / 1234)

![Started Linux](/shell1.png)

## Installing Pi-Hole
```bash
curl -sSL https://install.pi-hole.net | bash
```
![PiHole](/pihole1.png)

Works (so far)
![PiHole](/pihole2.png)

## Upgrading system
```bash
apt-get upgrade
```
About 50 packages or so are upgraded.

## Known issues
* Wifi not working.

## Troubleshootings
* You can attach a keyboard and mouse to the USB ports. The Logitech receiver is also working such that I could use my keyboard.
