#### Downloading configured Raspbian image

Navio2 requires a preconfigured Raspbian to run. We provide a unified SD card image for Raspberry Pi 2 and 3. The OS is headless, i.e. it comes without GUI as it is not required for drone applications.

[Emlid Raspbian Image](https://files.emlid.com/images/emlid-raspbian-20160718.img.xz), [(md5)](https://files.emlid.com/images/MD5SUMS)

[Emlid Raspbian Beta Image with ROS and WBC support](https://files.emlid.com/images/emlid-raspbian-20161229.img.xz), [(md5)](https://files.emlid.com/images/MD5SUMS)

#### Writing image to SD card

* Get the latest Emlid Raspbian Image.
* Download, extract and run [Etcher](https://etcher.io/) with administrator rights.
* Select the archive file with image and sd card drive letter.
* Click “Flash!”. The process may take a few minutes.

<iframe  title="Emlid manuals" width="680" height="380" src="https://www.youtube.com/embed/i8_TFYWYt_M" allowfullscreen></iframe>

More detailed instructions are available [here](http://www.raspberrypi.org/documentation/installation/installing-images/).

#### Configuring Wi-Fi access

Raspberry Pi3 has an internal Wi-Fi module, while Raspberry Pi2 requires an external USB Wi-Fi dongle. An extensive list of supported dongles is available [here](http://elinux.org/RPi_USB_Wi-Fi_Adapters).

Wi-Fi networks can be configured by editing the /boot/wpa_supplicant.conf file located on SD card. To add your network simply add the following lines to it:

```bash
network={
  ssid="yourssid"
  psk="yourpasskey"
  key_mgmt=WPA-PSK
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

**wpa_passphrase on Linux**

If you edit the file on a Raspberry or on a Linux computer you can populate **wpa_supplicant.conf** with a utility called **wpa_passphrase** like this:

```sudo bash -c "wpa_passphrase SSID password >> /boot/wpa_supplicant.conf```

#### Upgrading

If required you can now upgrade your system by running:

```sudo apt-get update && sudo apt-get dist-upgrade```
