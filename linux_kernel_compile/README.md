# linux kernel compile
You can issue the default configuration process by running `make xxx_defconfig` and that make target is a file in the folder `arch/arm/configs/`. These default configurations are not designed to exactly fit your target, but are rather meant to be a superset so you only have to modify them a bit.

The `make xxx_defconfig` creates your initial `.config`, which you can now edit through `make menuconfig` and make your changes. After that, you can run `make` which will then compile the kernel using your settings.

