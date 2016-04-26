#### Downloading configured Raspbian image

We provide an SD card image of Raspbian for usage in drone and research applications.
Please only use this image with Navio as it has been specially configured for it.

[Emlid Raspbian Image for Navio2/Navio+ (emlid-raspbian-20160408)](https://files.emlid.com/images/emlid-raspbian-20160408.img.xz)

<sub> Older image (20160212) is available for download [here](https://files.emlid.com/images/emlid-raspberrypi2-raspbian-navio2-20160212.img.xz). Please use only if necessary, otherwise use the image above. </sub>

#### Writing image to SD card

**On Windows:**

* Extract an image (using 7-Zip or other unpacker).
* Download and extract [Win32DiskImager utility](http://sourceforge.net/projects/win32diskimager/)
* Run Win32DiskImager utility with administrator rights.
* Select the extracted image file and sd card drive letter.
* Click “Write”. The process may take a few minutes.

**On Linux and Mac OS:**

Extract an image.  
For Ubuntu\Linux run:
```bash
unxz emlid-raspbian-20160408.img.xz
```
This will result in an uncompressed image.  
Unmount SD card partitions if they were mounted.
Run

For Ubuntu\Linux:
```bash
sudo dd bs=1M if=emlid-raspbian-20160408.img of=/dev/mmcblk0
```

For Mac OS:

* Find the memory card using `diskutil list` command(Try running it with and without the card inserted).
It will be one of the /dev/diskX instances.
**Be careful with the number as you might destroy your whole OS X installation.**
* Unmount the disk with `sudo diskutil unmountDisk /dev/diskX`
* Write the image with
```bash
sudo dd bs=1m if=emlid-raspbian-20160408.img of=/dev/rdiskX
```
Notice the addition of r in the disk path. It shows that that's this device is not buffered and will make the writing procedure much faster.
The process may take a few minutes, after it’s finished dd will display a message.

More detailed instructions are available [here](http://www.raspberrypi.org/documentation/installation/installing-images/).

#### Configuring Wi-Fi access

There are a few ways to configure Raspberry Pi to connect to your WiFi network. First, it is necessary to connect a supported USB dongle. Raspberry Pi supports a lot of WiFi dongles, the most common ones are based on RTL8192\8188 chipsets, Ralink chips are also widely supported. An extensive list of supported dongles is available [here](http://elinux.org/RPi_USB_Wi-Fi_Adapters).

Wi-Fi networks can be configured by editing the /boot/wpa_supplicant.conf file located on SD card. To add your network simply add the following lines to it:

```bash
network={
  ssid="yourssid"
  psk="yourpasskey"
}
```

To get access to this file use one of the following methods:

**Edit configuration on SD card**

Simply plug an SD card. After getting access to SD card contents, open /boot/wpa_supplicant.conf (with root privileges on Linux) and edit the file as described above.

**Use monitor and keyboard**

Connect HDMI monitor and USB keyboard to your Raspberry, power it up and you will get access to the console, where you can use text editor to modify wpa_supplicant. After logging into the system, type:

```bash
sudo nano /boot/wpa_supplicant.conf
```

Modify the file as described above, save it and reboot.
This method may be problematic because some keyboards are not compatible with this kernel. If your keyboard does not work, try another one or use another method.

**Use ethernet**

You can connect to Raspberry Pi over Ethernet by plugging it using Ethernet cable to a switch, router or directly to your computer.

**Finding an IP address**

To find an IP address of your Raspberry Pi use nmap utility.

It can be run from the console on your desktop:
nmap -sn 192.168.1.*

You can use it with a GUI such as Zenmap or Fing application on your phone.

Look for the hostname ”navio”.


#### Known Issues


This section lists issues **we’re currently working on**. This is not a comprehensive list of known issues, simply the ones that users are most likely to encounter.

1. Erratic PWM appears when motorised gimball is used ([here](https://community.emlid.com/t/auxiliary-pwm-channels-for-navio-2). This is fixed for most of the cases. Refer to [this section](http://docs.emlid.com/navio2/Navio-APM/installation-and-running/#further-configuration).

2. Internal compass offsets are too high (take a look at [these topics](https://community.emlid.com/search?q=compass%20variance))


If you have another problem, feel free to visit our [forum](https://community.emlid.com/) to find a solution or ask a question.
