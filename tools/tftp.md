# Beaglebone Black Network Boot

## TFTP introduction, installation

TFTP is a popular method of booting IP phones, routers, and net boot capable PCs. It provides a simple way to download files to clients. TFTP requires a server to be running somewhere on the network, though.

I will use `tftp-hpa` here.
if you have not installed it,

run `sudo apt-get install tftp-hpa tftpd-hpa` if you are using debian beased linux distribution

## Configuration

### set path for TFTP

`sudo nano /etc/default/tftpd-hpa`

```sh
# /etc/default/tftpd-hpa
TFTP_USERNAME=”tftp”
TFTP_DIRECTORY=”/home/USERNAME/rootfs”
TFTP_ADDRESS=”0.0.0.0:69″
TFTP_OPTIONS=”–secure”
```

### set path permission

We need to change the ownership, and permissions for the NFS share directory so TFTP can do its job properly.

```sh
sudo chown nobody\: /home/USERNAME/rootfs
sudo chmod 777 /home/USERNAME/rootfs
```

## TFTP service test

```sh
sudo service tftpd-hpa restart
cd ~
tftp 127.0.0.1
```

Do make sure to issue the command cd `~` to properly test TFTP.

```sh
>get /boot/am335x-boneblack.dtb
>quit
ls
```

make sure am335x-boneblack.dtb exist in current working directory. Then remove am335x-boneblack.dtb.

```sh
rm am335x-boneblack.dtb
```

## Beaglebone Black – uEnv.txt

show my version first.

```sh
#uEnv.txt boot kernel from TFTP, NFS or SD

#basic setting for booting from tftp
ipaddr=192.168.8.10
serverip=192.168.8.1
gatewayip=192.168.8.254
staticip=${ipaddr}:${serverip}:${gatewayip}:255.255.255.0:::off

#setting for kernel loading
kloadaddr=0x82000000
fdtaddr=0x83000000

##Disable HDMI/eMMC
cape_disable=capemgr.disable_partno=BB-BONELT-HDMI,BB-BONELT-HDMIN,BB-BONE-EMMC-2G
#cape_enable=capemgr.enable_partno=pru-enable

#parameters for kernel
mmc_args=setenv bootargs console=ttyO0,115200n8 quiet root=/dev/mmcblk0p2 ro rootfstype=ext4 rootwait ${cape_disable} ${cape_enable}
nfs_args=setenv bootargs console=ttyO0,115200n8 root=/dev/nfs rw nfsroot=${serverip}:${rootpath} ip=${staticip} ${cape_disable} ${cape_enable}


#load kernel and device tree
load_u_kernel=tftpboot ${kloadaddr} /boot/uImage
load_z_kernel=tftpboot ${kloadaddr} /boot/zImage
loadfdt=tftpboot ${fdtaddr}  /boot/dtbs/am335x-boneblack.dtb


#booting options
boot_uImage=bootm ${kloadaddr} - ${fdtaddr}
boot_zimage=bootz ${kloadaddr} - ${fdtaddr}

#to boot
uenvcmd=run load_u_kernel; run loadfdt; run mmc_args; run boot_uImage

#uenvcmd=run load_z_kernel; run loadfdt; run mmc_args; run boot_zimage

```

### demo here

```sh
bootdelay=1
ipaddr=xxx.xxx.xxx.xxx
serverip=xxx.xxx.xxx.xxx
static_ip=xxx.xxx.xxx.xxx:xxx.xxx.xxx.xxx:xxx.xxx.xxx.xxx:255.255.255.0:arm
rootpath=/home/username/rootfs,rsize=16384,wsize=16384
console=ttyO0,115200n8
loadtftp=tftpboot 0x80200000 /boot/uImage; tftpboot 0x815f0000 /boot/am335x-boneblack.dtb
netargs=setenv bootargs console=${console} ${optargs} root=/dev/nfs nfsroot=${serverip}:${rootpath},vers=3 rw ip=${static_ip}
uenvcmd=setenv autoload no; run loadtftp; run netargs; bootm 0x80200000 – 0x815f0000
```

### detail here

```sh
bootdelay=1
ipaddr=xxx.xxx.xxx.xxx
serverip=xxx.xxx.xxx.xxx
```

Technically `bootdelay=1` is not needed. `ipaddr=xxx.xxx.xxx.xxx`, and `serverip=xxx.xxx.xxx.xxx` are needed so uboot knows where to look for the Linux image, and device tree blob for the Beaglebone Black. If these parameters are omitted, the Beaglebone Black will hang with 3 USR LEDs lit solid shortly after starting the boot process.

```sh
static_ip=xxx.xxx.xxx.xxx:xxx.xxx.xxx.xxx:xxx.xxx.xxx.xxx:255.255.255.0:arm
```

Translates to:

```sh
static_ip=ipaddr:serverip:gatewayip:netmask:hostname
```

All of these parameters with the exception of hostname are necessary with `uEnv.txt` in its current configuration. This lets uboot know where to find the /root file system. Or more correctly, these parameters are passed to the kernel through `${optargs}`.

### reference

http://wiki.beyondlogic.org/index.php/BeagleBoneBlack_Building_Kernel

http://www.embeddedhobbyist.com/2013/06/beaglebone-black-network-boot/










