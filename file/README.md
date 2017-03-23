Mount `img` file in Linux

1. Check 'disk' structure by `fdisk`:

![](/assets/check_img.png)

1. Calculate the offset: `offset = START_UNIT * SECTOR_SIZE`
