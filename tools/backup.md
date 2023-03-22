# backup
## Data

### backup

``` sh
ssh root@remoteIP "dd if=/dev/mmcblk0p1" | gzip -1 - | dd of=boot.img.gz
```

reading and transfer at remote machine to reduce the CPU pressure and make the whole processs finish in a short time.

### restore

For the restore process, it's better to do it locally

``` sh
gzip -dc path/to/boot.img.gz | sudo dd of=/dev/mmcblk0p1
```

## MBR & partation table (Schema)

### Backing up the MBR

The MBR is stored in the the first 512 bytes of the disk. It consist of 3 parts:
The first 446 bytes contain the boot loader.
The next 64 bytes contain the partition table (4 entries of 16 bytes each, one entry for each primary partition).
The last 2 bytes contain an identifier
To save the MBR into the file "mbr.img":
`dd if=/dev/hda of=/mnt/sda1/mbr.img bs=512 count=1`

To restore (be careful: this could destroy your existing partition table and with it access to all data on the disk):
`dd if=/mnt/sda1/mbr.img of=/dev/hda`

If you only want to restore the boot loader, but not the primary partition table entries, just restore the first 446 bytes of the MBR:

`dd if=/mnt/sda1/mbr.img of=/dev/hda bs=446 count=1`

To restore only the partition table, one must use

`dd if=/mnt/sda1/mbr.img of=/dev/hda bs=1 skip=446 count=64`

You can also get the MBR from a full dd disk image.
`dd if=/path/to/disk.img of=/mnt/sda1/mbr.img bs=512 count=1`

### backup the partation table of a disk

```bash
sfdisk -d /dev/mmcblk0  > table.bak
```

### restore a partition table file to a disk

``` shell
sfdisk /dev/mmcblk0 <  table.bak
```

show schema:
`sfdisk -l /dev/mmcblk0`

```shell
partition table of /dev/mmcblk0
  │unit: sectors

  │/dev/mmcblk0p1 : start=     2048, size=   196608, Id= e, bootable
  │/dev/mmcblk0p2 : start=   198656, size=  7016448, Id=83
  │/dev/mmcblk0p3 : start=        0, size=        0, Id= 0
  │/dev/mmcblk0p4 : start=        0, size=        0, Id= 0
```
