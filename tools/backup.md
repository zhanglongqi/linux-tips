# backup
####backup remote disk partition to local
``` sh
ssh root@remoteIP "dd if=/dev/mmcblk0p1" | gzip -1 - | dd of=boot.gz
```
reading and transfer at remote machine to reduce the CPU pressure and make the whole processs finish in a short time.

#### restore image to disk partition
For the restore process, it's better to do it locally
``` sh
gzip -dc path/to/boot.gz | sudo dd of=/dev/mmcblk0p1
```

