+++
date = "2016-08-20T20:58:55-06:00"
title = "dell sputnik developer edition: sluggish performance"
categories = ["linux", "ubuntu", "tech-support"]
+++

After upgrading to Ubuntu 16.04 on my 2013 [Dell Sputnik Developer Edition](https://www.dell.com/developers) ultrabook I was experiencing extremely sluggish performance.  It has an 17 processor and 8 GB of RAM but sometimes just loading up 2 or 3 tabs in Firefox would slow it to a halt.  I knew I wasn't running anything incredibly memory intensive so I figured the problem had to be cpu bound.
I checked out lscpu to see what frequency the cpu was running at.
~~~
$ lscpu | grep "CPU"
CPU op-mode(s):        32-bit, 64-bit
CPU(s):                4
On-line CPU(s) list:   0-3
CPU family:            6
Model name:            Intel(R) Core(TM) i7-3537U CPU @ 2.00GHz
CPU MHz:               800.0000
CPU max MHz:           3100.0000
CPU min MHz:           800.0000
NUMA node0 CPU(s):     0-3
~~~
<br />

Notice that the CPU MHz is at the minimum frequency.  Intel has a CPU throttling feature called SpeedStep that will throttle your clock speed up and down dynamically depending on the current load.  In order to spike my cpu and get all the cores pegged I opened up a few terminals and ran:
~~~
$ while :; do :; done                                              
~~~
<br />
Re-run lspcu:
~~~
$ lscpu | grep "CPU MHz"
CPU MHz:               800.0000
~~~
<br />
It's still running at the minimum frequency, clearly the throttling isn't working correctly.

After a little bit of googling I figured out that I needed to install the proprietary intel microcode driver.
~~~
$ sudo apt-get install intel-microcode
~~~
<br />

After installing that package the CPU throttling started working correctly and the sluggishness I was experiencing all went away.
<!--more-->
