####OTG connection

Connect USB NEO6-T dongle using OTG cable to and Android device. Your phone or tablet should power the device, if not - use OTG cable with external power.

![otg](RTKLIB/img/RTKGPS+OTG.jpeg)

####Configuration
First we will try out single mode in which RTKGPS+ receives raw data from NEO6-T dongle and calculates coordinates. 

**Enable rover and configure as an the picture**

![enable](RTKLIB/img/RTKGPS+EN.png)

**Edit startup commands**

It is imortant to configure NEO-6T to output raw data.

![config](RTKLIB/img/RTKGPS+CMD.png)

**Observe satellite SNR**

Signal to noise ration is the most imortant metric for RTK performance. You shoud have 4-5 satellites close to 50 dB/Hz.

![config](RTKLIB/img/RTKGPS+SV.png)

