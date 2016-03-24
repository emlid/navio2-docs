#### Video Streaming with Navio2

Streaming real-time video from a drone powered by a Raspberry Pi 2 has never been easier.  There is only a handful of actions that you need to make to get a drone streaming real-time video to a remote PC, tablet, phone or whatnot.

#### Hardware

This instructions are for Raspberry Pi Camera Module.
Please note that Raspberry Pi Camera Module emits a lot of RF noise which may affect GPS performance. To workaround that wrap Camera Module and its cable using tape and alumnium\copper foil (use tape to keep foil from short curcuiting Camera Module pcb).

#### Raspberry PI2

First things first. You need to _expand filesystem_ and _enable camera_ using Raspberry Pi configuration tool. Type the following command in console:
```bash
sudo raspi-config
```  
Raspi-config menu will appear on your screen. After changeing the options press the Finish button. Expand filesystem and enable camera options require a reboot to take effect. Raspi-config will ask if you wish to reboot now when you select the Finish button.

Now you need install gstreamer1.0 package on RPi2:  
```bash
sudo apt-get update
sudo apt-get install gstreamer1.0
```
This operation takes a long time. 

After the installation has completed you can choose whatever platform you want to stream FPV to.

#### Ubuntu

If you're going to stream to a Ubuntu PC, install the same packages locally.

```bash
sudo apt-get install gstreamer1.0
```

#### Android

Download and install QtGStreamerHUD:
[QtGStreamerHUD.apk](http://files.emlid.com/QtGStreamerHUD.apk). Find out your IP address in the Preferences. You'll need it in order to connect to the phone from your RPi2. Please ensure Unknown sources is enabled in settings before installing. After completing the installation , hit the gears icon on the top right and set the pipeline to "udpscrc port=9000 buffer-size=600....." the second option in the pipeline dropdown menu.

Use our [our](http://docs.emlid.com/Navio-APM/installation-and-running/) tutorial to run APM using the IP you just found out.

Here's the app in action

![selfie](img/gstreamer-selfie.jpg)

Unfortunately, the cable length was not enough to make a selfie but at least we'd tried.

#### Mac OS X

The simplest way is to use brew. To install it run the following in your Mac terminal:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install gstreamer gst-libav gst-plugins-ugly gst-plugins-base gst-plugins-bad gst-plugins-good
```

#### Windows

Download and install [gstreamer for Windows](http://gstreamer.freedesktop.org/data/pkg/windows/1.4.5/gstreamer-1.0-x86_64-1.4.5.msi).

#### Launching

Now everything is ready for streaming.

***On your computer***

```bash
gst-launch-1.0 -v udpsrc port=9000 caps='application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264' ! rtph264depay ! avdec_h264 ! videoconvert ! autovideosink sync=f
```
From now on, your computer will be waiting for the input stream from Raspberry PI2. Once it gets a stream, you'll see the real-time video from your drone.

***On Raspberry***

```bash
raspivid -n -w 1280 -h 720 -b 1000000 -fps 15 -t 0 -o - | gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=10 pt=96 ! udpsink host=<remote_ip> port=9000
```
where ```<remote_ip>``` is the IP of the device you're streaming to.

Adjust bitrate with ***-b*** switch or ***-fps*** if your video lags behind.

Feel free to ask on our [forum](http://community.emlid.com) if you stumble upon any problems. We're always there at your convenience.  
