# compile setting
首先，配置时可能出现的选项，对其选择先来个说明吧。
```
Typically, your choices for each option are shown in the format [Y/m/n/?] The capitalized   letter is the default, and can be selected by just pressing the Enter key. The four choices are:
```

```bash

        y    Build directly into the kernel.

        n    Leave entirely out of the kernel.

        m   Build as a module, to be loaded if needed.

        ?    Print a brief descriptive message and repeat the prompt.
```
y表示是(相应功能将直接编译进内核），m表示模块(相应功能将编译为一个模块，在需要时加载)，以及n表示否(相应功能不会包含进内核)。?则（对该配置项）打印出简要的描述信息并重复刚才的选择提示。

其次，我使用的最多的两个配置命令分别是：`make muneconfig`和`make oldconfig`

`make oldconfig`和`make config`类似，但是它的作用是在现有的内核设置文件基础上建立一个新的设置文件，只会向用户提供有关新内核特性的问题，在新内核升级的过程 中，`make oldconfig`非常有用，用户将现有的配置文件`.confi`g复制到新内核的源码中，执行`make oldconfig`，此时，用户只需要回答那些针对新增特性的问题。

`make menuconfig`基于终端的一种配置方式，提供了文本模式的图形用户界面，用户可以通过光标移动来浏览所支持的各种特性。使用这用配置方式时，系统中必须安装有`ncurese`库。


在内核树的根目录中，有一个`.config`文件，它记录了内核的配置选项，可直接对它进行修改，再运行。在.config文件中，每个配置和选项的值只能为”y”和”m”两者之一，如果不需要这个特性不再支持她，那么可以将对应的选项用”#”注释掉。实际上,如果你手头有合适的`.config`文件,可以运行`make oldconfig` 直接按`.config`的内容来配置`$ sudo make oldconfig`
对内核的配置都是围绕`.config`来展开的. 即便开始`.config`文件不存在,进行配置后会创造它.

一般来说，内核配置保存于`/usr/src/linux-*/.config`文件中。在`/boot/config-<版本>`有其备份。请保留它以备后用。



常见的几种配置方式：

为了完成内核的配置，必须切换到root用户，然后转入内核源码目录(就是你下载新内核的目录)：

`#cd /usr/src/linux/linux-2.6.38`

然后执行下面命令之一:

`#make config`

`#make oldconfig`

`#make menuconfig`

`#make gconfig`

`#make defconfig`

`#make allyesconfig`

`#make allmodconfig`


1.**make config**


基于文本的最为传统的也是最为枯草的一种配置方式，但是它可以使用任何情况，这种方式会为每一个内核支持的特性向用户提问，如果用户回答“y”，则把特性编译进内核；回答“m”，则它特性作为模块进行编译；回答“n”，则表示不对该特性提供支持

如果回答每个问题前，必须考虑清楚，如果在配置过程中犯了错误给了错误的回答，就只能按“ctcl+c”强行退出了



2.**make oldconfig**

`make oldconfig`和`make config`类似，但是它的作用是在现有的内核设置文件基础上建立一个新的设置文件，只会向用户提供有关新内核特性的问题，在新内核升级的过程 中，`make oldconfig`非常有用，用户将现有的配置文件`.config`复制到新内核的源码中，执行`make oldconfig`，此时，用户只需要回答那些针对新增特性的问题

`make silentoldconfig : Like above, but avoids cluttering the screen with questions already answered.`和上面oldconfig一样，但在屏幕上不再出现已在.config中配置好的选项。



3.**make menuconfig**

基于终端的一种配置方式，提供了文本模式的图形用户界面，用户可以通过光标移动来浏览所支持的各种特性。使用这用配置方式时，系统中必须安装有`ncurese`库，否则会显示`Unable to find the Ncurses libraies`的错误提示



4.**make xoncifg**

基 于X Winodws的一种配置方式，提供了漂亮的配置窗口，不过只有能够在X Server上使用root用户欲行X应用程序时，才能够使用，它依赖于QT，如果系统中没有安装QT库，则会出现`Unable to find the QT installation`的错误提示



5.**make gconfig**

与`make xocnifg`类似，不同的是`make gconfig`依赖于GTK库



6.**make defconfig**

按照默认的配置文件arch/i386/defconfig对内核进行配置，生成.config可以用作初始化配置，然后再使用make menuconfig进行定制化配置



7.**make allyesconfig**

尽量多地使用“y”设置内核选项值，生成的配置中包含了全部的内核特性

`make allnoconfig` :除必须的选项外,其它选项一律不选. (常用于嵌入式系统).


8.**make allmodconfig**

尽可能多的使用“m”设置内核选项值来生成配置文件



下载好Linux内核源代码后，在源代码的根目录执行

`make localyesconfig`或者`make localmodconfig`

然后系统就会根据你的硬件自动生成一个适应你的硬件的`.config` (内核的配置文件)

`make localmodconfig`会执行`lsmod`命令查看当前系统中加载了哪些模块(Modules)，并最后将原来的`.config`中不需要的模块去掉，仅保留前面`lsmod`出来的这些模块，从而简化了内核的配置过程。

这样做确实方便了很多，但是也有个缺点：该方法仅能使编译出的内核支持当前内核已经加载的模块。因为该方法使用的是lsmod的结果，如果有的模块当前没有加载，那么就不会编到新的内核中。

`There’s an additional “make localyesconfig” target, in case you don’t want to use modules and/or initrds.`



几条好的建议：

除非您使用初始化ramdisk (initrd)，否则绝不要把挂载根文件系统必需的驱动程序(硬件驱动以及文件系统驱动)编译成模块！而如果您确实使用初始化ramdisk，请为ext2FS支持选项选择Y，因为ramdisk使用该文件系统。您还需要initrd支持。
如果您系统中有网卡，将它们的驱动编译成模块。这样，您就能够在/etc/modules.conf中用别名定义哪一块网卡第一，哪一块第二，等等。如果您将驱动程序编译进了内核，它们加载的顺序将取决于当初它们链接进内核的顺序，而这不一定是您想要的。
最后，如果您不清楚某个选项的含义，请阅读其帮助！而如果该帮助信息依然不能解决您的困惑，请保留该选项原来的样子。(在config和oldconfig中可以按?键访问帮助。)
配置最终结束后，请保存您的配置并退出。

[原文]在这里
[原文]:http://www.51testing.com/html/38/225738-237838.html
