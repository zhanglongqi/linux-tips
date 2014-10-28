#uImage、zImage、bzImage、vlinzx区别

在网络中，不少服务器采用的是Linux系统。为了进一步提高服务器的性能，可能需要根 据特定的硬件及需求重新编译Linux内核。编译Linux 内核，需要根据规定的步骤进行，编译内核过程中涉及到几个重要的文件。比如对于RedHat Linux，在/boot目录下有一些与Linux内核有关的文件 .
　　编译过RedHat Linux内核的人对其中的System.map、vmlinuz、initrd-2.4.7-10.img印象可能比较深刻，因为编译内核过程中涉及到这些文件的建立等操作。那么这几个文件是怎么产生的?又有什么作用呢?

对于Linux内核，编译可以生成不同格式的映像文件，例如：
`make zImage`
`make uImage`
`make bzImage`
##zImage

是ARM Linux常用的一种压缩映像文件不能超过512KB，`bzImage` 即`bigzImage` ，二者的内核都是`gzip`压缩的

##uImage

是U-boot专用的映像文件，它是在`zImage之前加上一个长度为0x40的“头”，说明这个映像文件的
类型、加载位置、生成时间、大小等信息。换句话说，如果直接从uImage的0x40位置开始执行，zImage和
uImage没有任何区别。另外，Linux2.4内核不支持uImage，Linux2.6内核加入了很多对嵌入式系统的支持
，但是uImage的生成也需要设置。

##vmlinuz

　　`vmlinuz`是可引导的、压缩的内核。“vm”代表“Virtual Memory”。Linux
　　支持虚拟内存，不像老的操作系统比如DOS有640KB内存的限制。Linux能够使用硬盘空间作为虚拟内存，因此得名“vm”。
　　`vmlinuz`是可执行 的Linux内核，它位于`/boot/vmlinuz`，它一般是一个软链接，比如图中是vmlinuz-2.4.7-10
　　的软链接。


vmlinuz的建立有两种方式:


    一 是编译内核时通过`make zImage`创建，手动拷贝到/boot目录下面. zImage适用于小内核的情况，它的存在是为了向后的兼容性.

　　二 是内核编译时通过命令`make bzImage`创建，然后手动拷贝至/boot目录下。`bzImage`是压缩的
　　内核映像，需要注意，`bzImage`不是用`bzip2`压缩的,bz表示`big zImage`。 `bzImage`中的b是“big”意思。 `zImage(vmlinuz)`和`bzImage(vmlinuz)`都是用gzip压缩的。它们不仅是一个压缩文件，而且在这两个文件的开头部分内嵌有`gzip`解压缩代码。所以你不能用`gunzip`或`gzip –dc`解包`vmlinuz`。
内核文件中包含一个微型的`gzip`用于解压缩内核并引导它。两者的不同之处在于，老的`zImage`解压缩内核到低端内存(第一个 640K)，`bzImage`解压缩内核到高端内存(1M以上)。如果内核比较小，那么可以采用`zImage`或`bzImage`之一，两种方式引导的系统运行 时是相同的。大的内核采用`bzImage`，不能采用`zImage`。
`vmlinux`是未压缩的内核，`vmlinuz`是`vmlinux`的压缩文件。

##initrd-x.x.x.img
　　`initrd`是“initial ramdisk”的简写。`initrd`一般被用来临时的引导硬件到实际内核`vmlinuz`能够接管并继续引导的状态。图中的initrd-2.4.7-10.img主要是用于加载ext3等文件系统及scsi设备的驱动。

　　比如，使用的是scsi硬盘，而内核vmlinuz中并没有这个scsi硬件的驱动，scsi模块是存储在根文件系统的/lib/modules下的，那么在装入scsi模块之前，内核不能加载根文件系统。为了解决这个问题，可以引导一个能够读实际内核的initrd内核并用initrd修正 scsi引导问题。initrd-2.4.7-10.img是用gzip压缩的文件，initrd实现加载一些模块和安装文件系统等功能。

`initrd`映象文件是使用`initrd`创建的。`initrd`实用程序能够创建`initrd`映象文件。这个命令是RedHat专有的(这也是为什么，在Linux内核包里/Documentation/Changes里面没有提到要将`initrd`升级）。其它Linux发行版或许有相应的命令。这是个很方便的实用程序。具体情况请看帮助:`man initrd`下面的命令创建`initrd`映象文件。
`initrd`映象文件是使用`initrd`创建的。`initrd`实用程序能够创建`initrd`映象文件。这个命令是RedHat专有的。其它Linux发行版或许有相应的命令。这是个很方便的实用程序。具体情况请看帮助:`man initrd`下面的命令创建`initrd`映象文件。

 ##uImage


vmlinux是内核文件，zImage是一般情况下默认的压缩内核映像文件，压缩vmlinux，加上一段解压启动代码得到。而uImage是u-boot使用bootm命令引导的Linux压缩内核映像文件格式,是使用工具mkimage对普通的压缩内核映像文件（zImage）加工而得。它是uboot专用的映像文件，它是在zImage之前加上一个长度为 64字节的“头”，说明这个内核的版本、加载位置、生成时间、大小等信息；其0x40之后与zImage没区别。
由于bootloader一般要占用0X0地址，所以，uImage相比zImage的好处就是可以和bootloader共存。
其实就是一个自动跟手动的区别,有了uImage头部的描述,u-boot就知道对应Image的信息,如果没有头部则需要自己手动去搞那些参数。

如何生成uImage文件？首先 在uboot的/tools目录下寻找mkimage文件，把其copy到系统/usr/local/bin目录下，这样就完成制作工具。然后在内核目录下运行make uImage，如果成功，便可以在arch/arm/boot/目录下发现uImage文件，其大小比zImage多64个字节。
此外，平时调试用uImage，不用去管调整了哪些东西；zImage则是一切OK后直接烧0X0。开机就运行。
