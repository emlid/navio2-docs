#### Downloading configured Raspbian image

We provide an SD card image of Raspbian configured for usage in drone applications. In research purposes you may use the default Raspbian, but if you’d like to use APM autopilot use only the image we provide.

[Emlid Raspbian Image for Navio2](http://files.emlid.com/images/emlid-raspberrypi2-raspbian-navio2-20160212.img.xz)

**Please do not perform apt-get upgrade on this image as it will overwrite our custom kernel. An image that will handle updates properly is in the works.**

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
unxz emlid-raspberrypi2-raspbian-navio2-20160212.img.xz
```
This will result in an uncompressed image.  
Unmount SD card partitions if they were mounted.
Run

For Ubuntu\Linux:
```bash
sudo dd bs=1M if=emlid-raspberrypi2-raspbian-navio2-20160212.img of=/dev/mmcblk0
```

For Mac OS:

* Find the memory card using `diskutil list` command(Try running it with and without the card inserted).
It will be one of the /dev/diskX instances.
**Be careful with the number as you might destroy your whole OS X installation.**
* Unmount the disk with `sudo diskutil unmountDisk /dev/diskX`
* Write the image with
```bash
sudo dd bs=1m if=emlid-raspberrypi2-raspbian-navio2-20160212.img of=/dev/rdiskX
```
Notice the addition of r in the disk path. It shows that that's this device is not buffered and will make the writing procedure much faster.
The process may take a few minutes, after it’s finished dd will display a message.

More detailed instructions are available [here](http://www.raspberrypi.org/documentation/installation/installing-images/).

#### Configuring Wi-Fi access

There are a few ways to configure Raspberry Pi to connect to your WiFi network. First, it is necessary to connect a supported USB dongle. Raspberry Pi supports a lot of WiFi dongles, the most common ones are based on RTL8192\8188 chipsets, Ralink chips are also widely supported. An extensive list of supported dongles is available [here](http://elinux.org/RPi_USB_Wi-Fi_Adapters).

Wi-Fi networks can be configured by editing the /etc/wpa_supplicant/wpa_supplicant.conf file located on SD card. To add your network simply add the following lines to it:

```bash
network={
  ssid="yourssid"
  key_mgmt=WPA-PSK
  psk="yourpasskey"
}
```

To get access to this file use one of the following methods:

**Edit configuration on SD card**

On Linux it’s easy, simply plug an SD card and it will be mounted.
On Windows you would need to install an ext4 driver like this one - http://www.paragon-software.com/home/extfs-windows/
On Mac OS you can also use Paragon’s software (http://www.paragon-drivers.com/extfs-mac/) or you can set up FUSE-ext.

After getting access to SD card contents open /etc/wpa_supplicant/wpa_supplicant.conf (with root privileges on Linux) and edit the file as described above.

**Create WiFi network with default ssid and psk**

By default in our image Raspberry Pi is set up to connect to the following WiFi network:

```bash
ssid = “emlidltd”
psk = “emlidltd”
key_mgmt = wpa-psk
```
You can rename your WiFi network, find your Raspberry, reconfigure wpa_supplicant and rename the network back.
Or, you can create a WiFi hotspot on your laptop.
Another options is to create new WiFi network using tethering on a smartphone.

**Use monitor and keyboard**

Connect HDMI monitor and USB keyboard to your Raspberry, power it up and you will get access to the console, where you can use text editor to modify wpa_supplicant. After logging into the system, type:

```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
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

Look for the hostname ”navio-rpi”.
