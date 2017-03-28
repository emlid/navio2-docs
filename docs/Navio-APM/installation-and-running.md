#### ArduPilot

You can run ArduPilot on Raspberry Pi 3 or 2 with Navio2. The autopilot's code works directly on Raspberry Pi. For ArduPilot to work properly please use the configured Raspbian distribution that we provide.  

#### Installing ArduPilot

Log in to your Raspberry Pi using SSH and install the package. It includes all vehicles and is based on the most stable branch available. Currently these are:

* ArduPlane 3.6.0
* ArduRover 3.0.1
* ArduCopter 3.4-rc1

```bash
pi@navio: ~ wget -qO - https://files.emlid.com/emlid-autopilot-gpg.key | sudo apt-key add -
pi@navio: ~ $ sudo apt-get update && sudo apt-get install apm-navio2
```
Navio2 is supported in ArduPilot upstream and if you'd like to build the binary yourself please proceed to the [Building from sources](building-from-sources.md).

#### Downloading stable binary

Also you can download the latest stable binary files from ardupilot buildserver. To download arducopter-quad binary:

```bash
pi@navio: ~ $ wget http://firmware.eu.ardupilot.org/Copter/stable/navio2-quad/arducopter-quad
pi@navio: ~ $ chmod +x arducopter-quad
```
In case of use another frame type, change tail of the link. For example `/navio2-hexa/arducopter-hexa`. Supported vehicle types are listed below.

#### Running ArduPilot

Supported vehicle types are:

* ArduRover
* ArduPlane
* ArduCopter-quad
* ArduCopter-tri
* ArduCopter-hexa
* ArduCopter-y6
* ArduCopter-octa
* ArduCopter-octa-quad
* ArduCopter-heli
* ArduCopter-single

To run ArduPilot binary type the following in your RPi's console (change ArduCopter-quad to your vehicle type):

```bash
pi@navio: ~ $ sudo ArduCopter-quad -A udp:192.168.1.2:14550
```
If you downloaded binary or build ArduPilot from sources launch obtained binary file:
```bash
pi@navio: ~ $ sudo ./arducopter -A udp:192.168.1.2:14550
```

Where 192.168.1.2 is the IP address of the device with the Ground Control Station - your laptop, smartphone etc.

Mapping between switches and serial ports (TCP or UDP can be used instead of serial ports):

* -A - serial 0 (always console; default baud rate 115200)  
* -C - serial 1 (normally telemetry 1; default baud rate 57600)  
* -D - serial 2 (normally telemetry 2; default baud rate 57600)  
* -B - serial 3 (normally 1st GPS; default baud rate 38400)  
* -E - serial 4 (normally 2st GPS; default baud rate 38400)  
* -F - serial 5  

Additionally take a look at [list of serial parameters](http://ardupilot.org/copter/docs/parameters.html?highlight=serial#serial-parameters) for Mission Planner.

When using UART for telemetry please keep in mind that serial ports have default baud rates.   
3DR Radios are configured for 57600 by default, so te simplest way to connect over them is to run with -C option.
If you would like to transfer telemetry over the UART port on Navio you can specify it like this:

```bash
pi@navio: ~ $ sudo ArduCopter-quad -C /dev/ttyAMA0
```

UDP and serial telemetry can be used simultaneously like this:

```bash
pi@navio: ~ $ sudo ArduCopter-quad -A udp:192.168.1.2:14550 -C /dev/ttyAMA0
```

#### Autostarting ArduPilot on boot

To automatically start ArduPilot on boot add the following (change -A and -C options to suit your setup) to /etc/rc.local file on your Raspberry Pi:

```bash
pi@navio: ~ $ sudo nohup ArduCopter-quad -A udp:192.168.1.2:14550 -C /dev/ttyAMA0 > /home/pi/startup_log &
```

#### Connecting to the GCS

**Mission Planner**

A Windows only ground station. It is the most feature complete, though.

**QGroundControl**

A crossplatform ground station for Mavlink-based flight stacks (like Ardupilot).

**APM Planner**

APM Planner is a ground station software for APM. It can be downloaded from the
[ardupilot.com](http://ardupilot.com/downloads/?category=35)

APM Planner listens on UDP port 14550, so it should catch telemetry from the drone automatically.

**MAVProxy**

MAVProxy is a console-oriented ground station software written in Python that can be used standalone or together with APM Planner. It’s well suited for advanced users and developers.

To install MAVProxy use [Download and Installation](http://ardupilot.github.io/MAVProxy/html/getting_started/download_and_installation.html) instructions.


To run it specify the --master port, which can be serial, TCP or UDP. It also can perform data passthrough using --out option.

```bash
mavproxy.py --master 192.168.1.2:14550 --console
```

Where 192.168.1.2 is the IP address of the GCS, not RPi.
