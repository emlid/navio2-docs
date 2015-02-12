#### APM

![apm](http://www.emlid.com/wp-content/uploads/2014/10/APM.png)

You can run APM (ArduPilot) on Raspberry Pi with Navio or Navio+. The autopilot's code works directly on Raspberry Pi using the APM's Linux HAL. Even though it is possible to run APM on standard Raspbian distribution it won't work properly as it requires lower latency. Please use Raspbian with real time kernel for running APM, you can get it in download section.

Important! Keep in mind that the code for Navio is in the experimental state. Use it with caution!

####State of the APM port to Raspberry Pi with Navio
These things were already supported in APM's library and worked with minimal configuration:

* MPU9250 inertial sensor
*  barometer
* Serial port

What has been added:

* Raspberry Pi build configuration
* Navio board configuration
* GPIO driver for Raspberry Pi
* RCOutput based on PCA9685 with 24.576 external oscillator
* RCInput - uses pigpio daemon to sample GPIOs with 1MHz rate, should be rewritten to work without pigpio
* RGB LED
* MPU9250 built-in compass driver
* U-blox GPS SPI driver
* ADC based on ADS1115 

####Installing APM
Log in to your Raspberry Pi using SSH or other method and download one of the ready to use APM binaries using wget.


For Navio or Navio+ pick one of the following corresponding to the type of your drone:

```bash
wget emlid.com/files/APM/Navio/APMrover2.elf
wget emlid.com/files/APM/Navio/ArduCopter.elf
wget emlid.com/files/APM/Navio/ArduPlane.elf
```



For Navio Raw pick one of the following corresponding to the type of your drone:

```bash
wget emlid.com/files/APM/NavioRaw/APMrover2.elf
wget emlid.com/files/APM/NavioRaw/ArduCopter.elf
wget emlid.com/files/APM/NavioRaw/ArduPlane.elf
```


If you'd like to build the binary yourself please proceed to the [Building from sources](building-from-sources.md).
####Running APM
To run APM binary type the following in your RPi's console (change APMrover2.elf to ArduCopter.elf or ArduPlane.elf if needed):

```bash
sudo ./APMrover2.elf -A udp:192.168.1.2:14550
```

Where 192.168.1.2 is the IP address of the device with the Ground Control Station - your laptop, smartphone etc.

Arguments specify serial ports (TCP or UDP can be used instead of serial ports) :

* -A is for telemetry
* -B is for external GPS
* -C is for secondary telemetry

If you would like to transfer secondary telemetry over the UART port on Navio you can specify it like this:

```bash
sudo ./APMrover2.elf -A udp:192.168.1.2:14550 -C /dev/ttyAMA0
```


####Autostarting APM on boot
To automatically start APM on boot add the following (change -A and -C options to suit your setup) to /etc/rc.local file on your Raspberry Pi:

```bash
sudo /home/pi/APMrover2.elf -A udp:192.168.1.2:14550 -C /dev/ttyAMA0 > /home/pi/startup_log &
```

####Connecting to the GCS
**APM Planner**

APM Planner is a ground station software for APM. It can be downloaded from the 
[ardupilot.com](http://ardupilot.com/downloads/?category=35)

APM Planner listens on UDP port 14550, so it should catch telemetry from the drone automatically.

**MAVProxy**

MAVProxy is a console-oriented ground station software written in Python that can be used standalone or together with APM Planner. It’s well suited for advanced users and developers. MAVProxy can be installed with pip:

```bash
pip install mavlink mavproxy console wp
```


To run it specify the --master port, which can be serial, TCP or UDP. It also can perform data passthrough using --out option.

```bash
<>mavproxy.py --master 192.168.1.2:14550 --console
```

Where 192.168.1.2 is the IP address of the GCS, not RPi.


