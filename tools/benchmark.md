# benchmark

## lmbench 3.0

### installation

Building lmbench3

To build lmbench3 for these experiments, we downloaded the source, extracted the files using untar, and changed into the lmbench3 directory using `cd`. The lmbench3 README explains that you can run the full suite of tests with the `make results` command.
Tip: There is a bug in the lmbench3 code (as noted in this kerneltrap.org thread for example: http://kerneltrap.org/node/14792).  We encountered this error when running the make command:

```shell
gmake[1]: *** No rule to make target '../SCCS/s.ChangeSet', needed by 'bk.ver'. Stop.

gmake[1]: Leaving directory '<dir>/lmbench3/src'

make: *** [lmbench] Error 2
```

We used the touch command to create the missing file to get the make to complete smoothly. From the lmbench3 directory, we entered the following:

```shell
mkdir ./SCCS

touch ./SCCS/s.ChangeSet
```

after that it will looks like this:

```shell
.
├── bin
│   └── armv7l-linux-gnu
├── doc
├── results
│   └── armv7l-linux-gnu
├── SCCS
│   └── s.ChangeSet
├── scripts
└── src
    └── webpage-lm

9 directories

```

Note that the default Makefile options were used for this article. Adding flags such as compiler optimization levels can cause some problems with the benchmark's accounting code, which can lead to inaccurate measurements.

### test

`cd src`

`make results`

this will compile the whole package and run the test.
it will takes more than one hour, so have have a cup of coffee first.

### generate report

`cd ../results/`

`make summary`

This is the result of my Beaglebone Black (rev C) running debian armhf:

```pre

                 L M B E N C H  3 . 0   S U M M A R Y
                 ------------------------------------
                 (Alpha software, do not distribute)

Basic system parameters
------------------------------------------------------------------------------
Host                 OS Description              Mhz  tlb  cache  mem   scal
                                                     pages line   par   load
                                                           bytes
--------- ------------- ----------------------- ---- ----- ----- ------ ----
B1        Linux 3.8.13+        armv7l-linux-gnu  995    15    64 1.0000    1
B1        Linux 3.8.13+        armv7l-linux-gnu  994    30    64 1.0000    1

Processor, Processes - times in microseconds - smaller is better
------------------------------------------------------------------------------
Host                 OS  Mhz null null      open slct sig  sig  fork exec sh
                             call  I/O stat clos TCP  inst hndl proc proc proc
--------- ------------- ---- ---- ---- ---- ---- ---- ---- ---- ---- ---- ----
B1        Linux 3.8.13+  995 1.30 2.47 3.77 9.35 21.6 1.07 5.22 893. 3063 7104
B1        Linux 3.8.13+  994 4.26 1.17 4.06 9.71 21.6 1.08 5.22 898. 6288 7153

Basic integer operations - times in nanoseconds - smaller is better
-------------------------------------------------------------------
Host                 OS  intgr intgr  intgr  intgr  intgr
                          bit   add    mul    div    mod
--------- ------------- ------ ------ ------ ------ ------
B1        Linux 3.8.13+ 1.0100 0.1000 6.1300   73.0   24.1
B1        Linux 3.8.13+ 1.0100 0.1000 6.1300   73.0   24.1

Basic float operations - times in nanoseconds - smaller is better
-----------------------------------------------------------------
Host                 OS  float  float  float  float
                         add    mul    div    bogo
--------- ------------- ------ ------ ------ ------
B1        Linux 3.8.13+ 8.9600   10.1   33.2  104.7
B1        Linux 3.8.13+   19.8   10.1   33.2  104.8

Basic double operations - times in nanoseconds - smaller is better
------------------------------------------------------------------
Host                 OS  double double double double
                         add    mul    div    bogo
--------- ------------- ------  ------ ------ ------
B1        Linux 3.8.13+ 8.9600   11.1   57.3  135.2
B1        Linux 3.8.13+ 8.9600   11.1   57.3  134.9

Context switching - times in microseconds - smaller is better
-------------------------------------------------------------------------
Host                 OS  2p/0K 2p/16K 2p/64K 8p/16K 8p/64K 16p/16K 16p/64K
                         ctxsw  ctxsw  ctxsw ctxsw  ctxsw   ctxsw   ctxsw
--------- ------------- ------ ------ ------ ------ ------ ------- -------
B1        Linux 3.8.13+   19.9   14.1   18.0   22.5  214.8    59.7   243.6
B1        Linux 3.8.13+   38.9   13.8   25.9   26.5  217.1    57.8   243.7

*Local* Communication latencies in microseconds - smaller is better
---------------------------------------------------------------------
Host                 OS 2p/0K  Pipe AF     UDP  RPC/   TCP  RPC/ TCP
                        ctxsw       UNIX         UDP         TCP conn
--------- ------------- ----- ----- ---- ----- ----- ----- ----- ----
B1        Linux 3.8.13+  19.9  35.4 43.5  57.6        82.1       242.
B1        Linux 3.8.13+  38.9  38.1 43.2  57.0        82.1       247.

File & VM system latencies in microseconds - smaller is better
-------------------------------------------------------------------------------
Host                 OS   0K File      10K File     Mmap    Prot   Page   100fd
                        Create Delete Create Delete Latency Fault  Fault  selct
--------- ------------- ------ ------ ------ ------ ------- ----- ------- -----
B1        Linux 3.8.13+                               12.9K 1.102 5.11830 7.303
B1        Linux 3.8.13+  112.8   88.5  153.8   86.9   12.9K 0.976 5.07480 7.308

*Local* Communication bandwidths in MB/s - bigger is better
-----------------------------------------------------------------------------
Host                OS  Pipe AF    TCP  File   Mmap  Bcopy  Bcopy  Mem   Mem
                             UNIX      reread reread (libc) (hand) read write
--------- ------------- ---- ---- ---- ------ ------ ------ ------ ---- -----
B1        Linux 3.8.13+ 309. 288. 124.  215.1  274.5  213.2  214.8 274. 1498.
B1        Linux 3.8.13+ 354. 221. 125.  219.4  274.3  214.4  214.4 274. 1498.

Memory latencies in nanoseconds - smaller is better
    (WARNING - may not be correct, check graphs)
------------------------------------------------------------------------------
Host                 OS   Mhz   L1 $   L2 $    Main mem    Rand mem    Guesses
--------- -------------   ---   ----   ----    --------    --------    -------
B1        Linux 3.8.13+   994 3.0580   11.8  215.6       425.9

```

http://www.bitmover.com/lmbench/
