# timeout setting
Sometimes the OS stop at the grub items even you set default timeout to a small positive number. Check your `
/boot/grub/grub.cfg` you can find
``` bash
if [ "${recordfail}" = 1 ] ; then
  set timeout=-1 #GRUB_RECORDFAIL_TIMEOUT
else
  if [ x$feature_timeout_style = xy ] ; then
    set timeout_style=menu
    set timeout=10
  # Fallback normal timeout code in case the timeout_style feature is
  # unavailable.
  else
    set timeout=10 #GRUB_TIMEOUT
  fi
fi
```
You will notice timeout is set at three statements, For the first one it's depends on the virable `recordfial` which will be found in `/boot/grub/grubenv`.
Changing the `grub.cfg` and `grubenv` is not recommanded because it will change back everytime after you update grub or caused by kernel updating.

Modify the templates the update using is my solution:

`GRUB_TIMEOUT=10` in `/etc/default/grub`

`set timeout=${GRUB_RECORDFAIL_TIMEOUT:--1}` in `/etc/grub.d/00_header`

Be care for the `--1`

