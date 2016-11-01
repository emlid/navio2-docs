#### Downloading configured Raspbian image

Navio2 requires a preconfigured Raspbian to run. We provide a unified SD card image for Raspberry Pi 2 and 3. The OS is headless, i.e. it comes without GUI as it is not required for drone applications.  

[Emlid Raspbian Image](https://files.emlid.com/images/emlid-raspbian-20160718.img.xz), [(md5)](https://files.emlid.com/images/MD5SUMS)

[Emlid Raspbian Beta Image with ROS support](https://files.emlid.com/images/emlid-raspbian-20161031.img.xz), [(md5)](https://files.emlid.com/images/MD5SUMS)

#### Writing image to SD card

**On Windows:**

* Extract an image (using 7-Zip or other unpacker).
* Download and extract [Rufus formatting utility](http://downloads.sourceforge.net/project/portableapps/Rufus%20Portable/RufusPortable_2.11.paf.exe?r=http%3A%2F%2Fportableapps.com%2Fapps%2Futilities%2Frufus-portable&ts=1474446747&use_mirror=kent).
* Run Rufus formatting utility with administrator rights.
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

Raspberry Pi3 has an internal Wi-Fi module, while Raspberry Pi2 requires an external USB Wi-Fi dongle. An extensive list of supported dongles is available [here](http://elinux.org/RPi_USB_Wi-Fi_Adapters).

Wi-Fi networks can be configured by editing the /boot/wpa_supplicant.conf file located on SD card. To add your network simply add the following lines to it:

```bash
network={
  ssid="yourssid"
  psk="yourpasskey"
}
```

To get access to this file use one of the following methods:

**Edit configuration on SD card**

Simply plug an SD card in your computer. After getting access to SD card contents, open /boot/wpa_supplicant.conf (with root privileges on Linux) and edit the file as described above.

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

#### Upgrading

If required you can now upgrade your system by running:

```sudo apt-get update && sudo apt-get dist-upgrade```
