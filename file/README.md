Mount `img` file in Linux

1. Check 'disk' structure by `fdisk`:

![](/assets/check_img.png)

1. Calculate the offset: `offset = SECTOR_SIZE * START_UNIT`
The offset in the example: `offset = 512`
![](/assets/mount_img.png)
