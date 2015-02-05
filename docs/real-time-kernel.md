# Real-Time kernel 
![](http://www.emlid.com/wp-content/uploads/2014/05/RT-Tests.png)
Linux kernel has configuration options that affect it's real-time capabilities. Default Raspbian kernel is compiled with CONFIG_PREEMPT option that allows all kernel code outside of spinlock-protected regions and interrupt handlers to be preempted by higher priority kernel threads. Real-time patch from Ingo Molnar adds CONFIG_PREEMPT_RT option that allows nearly all of the kernel code to be preempted, except for a few raw spinlock critical regions.


The histogram shows latency comparison between the default Raspbian kernel and the kernel with real-time patch.


We've used cyclictest program to obtain data with the following parameters:

```
cyclictest -l10000000 -m -n -a0 -t1 -p99 -i400 -h400 -q ```

Latencies for default Raspbian kernel (PREEMPT) in uS: Min: 00014 Avg: 00028 Max: 01247

Latency results for PREEMPT_RT patched kernel in uS: Min: 00012  Avg: 00027 Max: 00077


The most important characteristic here is the max latency which indicates the longest time it might take your Raspberry Pi to respond to an event. As it can be observed PREEMPT_RT kernel provides much lower latency thus making it more suitable for usage in time-sensitive embedded systems. It is important to remember that to make full use of real time capabilities of the kernel it is necessary to do proper prioritizing.


You can compile PREEMPT_RT real time kernel for Raspberry Pi by yourself or you can download modified 
 Raspbian SD card image that we prepared. The image was tested for compatibility with Raspberry Pi Model B and Raspberry Pi Model B+. The image is not intended for general purpose use but rather for embedded electronics projects.


~~UPDATE (10 SEP 2014) - Raspbian with RT-kernel for Raspberry Pi Model B+ (also compatible with Model B)~~

UPDATE (25 NOV 2014) - Raspbian with RT-kernel for Raspberry Pi Model A/B/A+/B+. The list of changes includes:

* Added RT_PREEMPT kernel(/boot/kernel-rt.img)
* Placed kernel source patched with rt-patch in /usr/src/
* Enabled SPI
* Enabled I2C
* Increased I2C speed to 400KHz
* Disabled serial console on UART port (/dev/ttyAMA0) so it can be used for telemetry
* Installed I2C-tools
* Installed pigpio (https://github.com/joan2937/pigpio)
* Installed rpi-serial-console (https://github.com/lurch/rpi-serial-console)
* Installed socat
* Installed screen
* Installed WiringPi
* Installed python-smbus
* Installed py-spidev

Known issues:

* Due to the required options some USB keyboards may not work<

Download links:

* <a href="http://www.emlid.com/files/Raspbian-RTkernel-2014-NOV.torrent" target="_blank">Raspbian-RTkernel-2014-NOV torrent</a>
* <a href="https://mega.co.nz/#!9Z4AlBoY!3_tou_JdQ-vflGiaYSUG3xixlhfEB0upNp19PvBSHds" target="_blank">Raspbian-RTkernel-2014-NOV on MEGA</a>

