# backup
##partation data backup

###backup remote disk partition to local
``` sh
ssh root@remoteIP "dd if=/dev/mmcblk0p1" | gzip -1 - | dd of=boot.gz
```
reading and transfer at remote machine to reduce the CPU pressure and make the whole processs finish in a short time.

### restore image to disk partition
For the restore process, it's better to do it locally
``` sh
gzip -dc path/to/boot.gz | sudo dd of=/dev/mmcblk0p1
```

##partation table (Schema) backup

###backup the partation table of a disk
```bash
sfdisk -d /dev/mmcblk0  > table.bak
```
###restore a partation table file to a disk
```
sfdisk /dev/mmcblk0 <  table.bak
```
the schema file:
```
partition table of /dev/mmcblk0
  │unit: sectors

  │/dev/mmcblk0p1 : start=     2048, size=   196608, Id= e, bootable
  │/dev/mmcblk0p2 : start=   198656, size=  7016448, Id=83
  │/dev/mmcblk0p3 : start=        0, size=        0, Id= 0
  │/dev/mmcblk0p4 : start=        0, size=        0, Id= 0
```
##MBR
```
dd if=/dev/sda of=/tmp/mbrsda.bak bs=512 count=1
```
