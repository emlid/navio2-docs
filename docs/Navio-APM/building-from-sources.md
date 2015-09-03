# Building from sources

![](http://www.emlid.com/wp-content/uploads/2014/10/APM.png)

####Where to get the code
Working repository for Navio APM port is on <a href="https://github.com/emlid/ardupilot" target="_blank" >our GitHub</a>.

Changes are being pushed in the main ArduPilot repository after some time.
#### How to build
You can build APM directly on your Raspberry Pi, it will take approximately 15 minutes.

For faster build environment it's better to use cross-compiling on Linux PC or virtual machine.

If you'd like to build on Raspberry Pi skip the next step.
####Cross-compiler setup on Linux (optional)
Standard ARM compiler from Ubuntu repositories is not suitable for cross-compiling for Raspberry Pi since it does not support hard float for ARMv6 architecture, instead use compiler provided by Raspberry Pi Foundation that is available <a href="https://github.com/raspberrypi/tools" target="_blank" >here</a>.


Download and extract it somewhere, for example in /opt/:

```bash
sudo git clone --depth 1 https://github.com/raspberrypi/tools.git /opt/tools
```


If you're using 32-bit distro add the following to your PATH:

```bash
export PATH=/opt/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin:$PATH
```

For 64-bit distro add this to your PATH

```bash 
export PATH=/opt/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin:$PATH
```

If you would like to add the compiler to the PATH permanently edit /etc/environment.

####Building APM
These steps are the same both for compiling APM directly on Raspberry Pi and cross-compiling.

Download the APM’s port for Navio:

```bash
git clone https://github.com/emlid/ardupilot.git
```

For Navio and Navio+ boards switch to the 'navio' branch:

```bash
git checkout navio
```

For Navio Raw switch to the 'navio-raw' branch:

```bash
git checkout navio-raw
```

Navigate to the autopilot’s directory, for example rover, run configuration and build it:

```bash
cd ardupilot/APMrover2
make configure
make navio
```
Build  for quadcopter:

```bash
cd ardupilot/ArduCopter
make configure
make navio
```

Build for hexacopter:

```bash
cd ardupilot/ArduCopter
make configure
make navio-hexa
```
To build for other frame types replace hexa with one of the following options:

```bash
quad tri hexa y6 octa octa-quad heli single obc nologging
```

Executable file will be placed in /tmp/APMrover2.build directory.

If you're using cross-compiler transfer the binary to your Raspberry Pi:

```bash
rsync -avz /tmp/APMrover2.build/APMrover2.elf pi@192.168.1.3:/home/pi/
```

Where 192.168.1.3 is an IP address of your Raspberry Pi with Navio.

If you are compiling directly on Raspberry copy the binary from tmp directory to your home folder:

```bash
cp /tmp/APMrover2.build/APMrover2.elf /home/pi/APMrover2.elf
```

Instructions how to run APM on Raspberry and to connect GCS to it are available  [here](installation-and-running.md).
