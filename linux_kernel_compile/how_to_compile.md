# How to compile kernel and modules (for Beaglebone Black)

Doing all the things in the kernel source root path:

## 1. generate the `.config`

The original configuration is used as basement to generate our own profile:

`cp arch/arm/configs/bb.org_defconfig defconfig_LQ`

`make menuconfig`

`load defconfig_LQ`
do some modification based on your needs then
`save defconfig_LQ`

`cp defconfig_LQ arch/arm/configs/beaglebone_defconfig`

`make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- beaglebone_defconfig`

## 2. compile the kernel

`make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- uImage dtbs`

maybe you can add `-j4` to make it faster.

## 3. compile the modules

`make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-  modules`

maybe you can add `-j4` to make it faster.

### 3.1. compile partial modules

`make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- SUBDIRS=drivers/input/touchscreen modules`

## 4. install modules

`sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-  modules_install`

`sudo` make sure you have the writing permission for the default path `/lib/modules/KERNEL-NAME`

or you can have no sudo and use:

`make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_PATH=/path/to/target modules_install`

or you can copy to target board directly:

`scp -r /lib/modules/3.8.13+ root@10.42.0.12:/lib/modules/`

if you make something wrong and want to recompile again, please `make clean` first.

[reference1] compile wiki

[reference2] compile wiki

[reference3], Beaglebone Black NFS root

[reference1]:http://wiki.beyondlogic.org/index.php/BeagleBoneBlack_Building_Kernel
[reference2]:http://elinux.org/Building_BBB_Kernel
[reference3]:http://www.embeddedhobbyist.com/beaglebone-black-nfs-root/
