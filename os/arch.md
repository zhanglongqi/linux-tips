# ARCH
###Is it armhf or armel?
By Jim Connors on Mar 16, 2013

Arm processors come in all makes and sizes, a certain percentage of which address a market where cost, footprint and power requirements are at a premium.  In this space, the inclusion of even a floating point unit would be considered an unnecessary luxury.  To perform floating point operations with these processors, software emulation is required.

Higher-end Arm processors come bundled with additional capability that enables hardware execution of floating point operations.  The difference between these two architectures gave rise to two separate Embedded Application Binary Interfaces or EABIs for ARM: soft float and VFP (Vector Floating Point).  Although there is forward compatibility between soft and hard float, there is no backward compatibility.  And in fact, when it comes to providing binaries for [Java SE Embedded for Arm]:http://www.oracle.com/technetwork/java/embedded/downloads/javase/index.html , Oracle provides two separate options: a soft float binary and a VFP binary.  In the Linux community, releases built upon both these EABIs are refereed to as armel based distributions.

Enter armhf.  Although a big step up in performance, the VFP EABI utilizes less-than-optimal argument passing when a floating point operations take place.  In this scenario, floating point arguments must first be passed through integer registers prior to executing in the floating point unit.  A new EABI, referred to as armhf optimizes the calling convention for floating point operations by passing arguments directly into floating point registers.  It furthermore includes a more efficient system call convention.  The end result is applications compiled with the armhf standard should demonstrate modest performance improvement in some cases, and significant improvement for floating point intensive applications.

Alas, armhf represents yet another binary incompatible standard, but one that has already gained considerable traction in the community. Although still relatively early, the transition from armel to armhf is underway.  In fact, Ubuntu has already announced that future releases will only be built to the armhf standard, effectively obsoleting armel. As mentioned in Henrik's Stahl's Blog, an armhf version of Java SE Embedded is in the works, and we have already made available a armhf-based developer Preview of JDK 8 with JavaFX: http://jdk8.java.net/fxarmpreview/javafx-arm-developer-preview.html.

In the interim, we will have to deal with the incompatibilities between armel and armhf.  Most recently we've seen a rash of failed attempts to run the ArmV7 VFP Java SE Embedded binary on top of an armhf-based Linux distro.  During diagnosis, the question becomes, how can I determine whether my Linux distribution is based on armel or armhf?  Turns out this is not as straightforward as one might think.  Aside from experience and anecdotal evidence, one possible way to ascertain whether you're running on armel or armhf is to run the following obscure command:

`readelf -A /proc/self/exe | grep Tag_ABI_VFP_args`

If the `Tag_ABI_VFP_args` tag is found, then you're running on an armhf system.  If nothing is returned, then it's armel.  To show you an example, here's what happens on a Raspberry Pi running the Raspbian distribution:

`readelf -A /proc/self/exe | grep Tag_ABI_VFP_args`

`Tag_ABI_VFP_args: VFP registers`

This indicates an armhf distro, which in fact is what Raspbian is.  On the original, soft-float Debian Wheezy distribution, here's what happens:

`readelf -A /proc/self/exe | grep Tag_ABI_VFP_args`

Nothing returned indicates that this is indeed armel.

Many thanks to the folks participating in this Raspberry Pi forum topic for providing this suggestion.

come from: https://blogs.oracle.com/jtc/entry/is_it_armhf_or_armel

