#####PREEMPT_RT
![](http://www.emlid.com/wp-content/uploads/2014/05/RT-Tests.png)

This is a release of Raspbian with real-time kernel for Raspberry Pi Model A+/B+. It is based on 2014-12-24-wheezy-raspbian with default kernel replaced to 3.12.36-rt50+ kernel and a few additional tunings.

Default Raspbian kernel is configured with PREEMPT option and provides worst case latency around single digit milliseconds. Real-time demanding applications require lower latencies than that. Real-time patch lowers the worst case latency to tens of microseconds, allowing for real-time applications such as autopilots to be run on Linux.

Results of testing with 

```
sudo cyclictest -l1000000 -m -n -a0 -t1 -p99 -i400 -h400 -q
```

```
PREEMPT    #Min: 00013uS #Avg: 00023uS #Max: 01153uS
PREEMPT_RT #Min: 00011uS #Avg: 00023uS #Max: 00066uS
```

Histogram values are on the plot above.

####List of changes

List of changes includes:

* Replaced default kernel with PREEMPT_RT kernel 3.12.36-rt50+
* Processor frequency set to 800MHz (can be changed in /boot/config.txt)
* Enabled camera, SPI, I2C and set its speed to 1MHz (if you’d like to connect a sensor with lower clock speed, edit the baudrate option in /etc/modprobe.d/i2c.conf)
* Installed pigpio, screen, socat, python-smbus, python-spi, cmake, cyclictest
* Disabled serial console, so /dev/ttyAMA0 UART can be used for radio (to enable back use raspi-config or edit /boot/cmdline.txt), disabled extra ttys
* Default WiFi network: ssid “emlidltd”, psk “emlidltd”, key_mgmt=WPA-PSK

**Source code**

* Patched and configured source code for Raspberry Pi real-time kernel is available on [GitHub](https://github.com/emlid/linux-rt-rpi)

####SD card image

* [emlid-raspberrypi1-raspbian-rt-2015feb.img.xz on MEGA](https://mega.nz/#!ocgGQDiI!QVPTZaNY0AiuPbcxQjOTmZ2un6d0j7W3g1jwheuotUc)

To flash the SD card refer to this [tutorial](http://elinux.org/RPi_Easy_SD_Card_Setup#Flashing_the_SD_Card_using_Windows)
