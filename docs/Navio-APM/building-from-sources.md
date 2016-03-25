![](http://www.emlid.com/wp-content/uploads/2014/10/APM.png)

#### Where to get the code

Navio2 is supported in the main [APM repository](https://github.com/diydrones/ardupilot).

#### How to build

APM binary for Navio2 can be built using two ways:

1) Directly on your Raspberry Pi. Simpler, but slower. Build takes approximately 15 minutes.

2) Using a cross-compiler (on Linux PC or virtual machine). This is much faster, but requires one-time setup.

If you'd like to build on Raspberry Pi or use Waf build system(either on Raspberry Pi or Linux PC) skip the next step.

#### Cross-compiler setup on Linux (optional)

Recommended compiler is the one that is provided by Raspberry Pi Foundation. Download and extract it somewhere, for example in /opt/:

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

#### Building APM using make-based build system

These steps are the same both for compiling APM directly on Raspberry Pi and cross-compiling.

Download the APM code and update submodules:

```bash
git clone https://github.com/diydrones/ardupilot.git
cd ardupilot
git submodule update --init
```

Navigate to the autopilot’s directory: APMrover, ArduCopter or ArduPlane. For example ArduCopter:

```bash
cd ArduCopter
```
Build for quadcopter:

```bash
make navio2-quad
```

To build for other frame types replace quad with one of the following options:

```bash
quad tri hexa y6 octa octa-quad heli single obc nologging
```

In the end of compilation binary file with the name ArduCopter.elf will be placed in the same directory.

If you're using cross-compiler transfer the binary to your Raspberry Pi:

```bash
rsync -avz ArduCopter.elf pi@192.168.1.3:/home/pi/
```

Where 192.168.1.3 is an IP address of your Raspberry Pi with Navio2.

#### Building APM using Waf build system

These steps are the same both for compiling APM directly on Raspberry Pi and cross-compiling.

Download the APM code and update submodules:

```bash
git clone https://github.com/diydrones/ardupilot.git
cd ardupilot
git submodule update --init
```  
Note that Waf should always be called from the ardupilot's root directory.

To keep access to Waf convenient, use the following alias from the root ardupilot directory:  
```bash
alias waf="$PWD/modules/waf/waf-light"
```  
Differently from the make-based build, with Waf there's a configure step to choose the board to be used:
```bash
waf configure --board=navio2
```

Now you can build arducopter: 
```bash
waf copter
```  
In the end of compilation binary file with the name ```arducopter``` will be placed in ```ardupilot/build/navio2/bin/``` directory.

If you're using cross-compiler transfer the binary to your Raspberry Pi:

```bash
rsync -avz ardupilot/build/navio2/bin/arducopter pi@192.168.1.3:/home/pi/
```

Where 192.168.1.3 is an IP address of your Raspberry Pi with Navio2.

For further information read [ardupilot WAF Build](https://github.com/diydrones/ardupilot/blob/master/README-WAF.md).


Instructions how to run APM on Raspberry Pi and to connect GCS to it are available in  [Installation and running section](installation-and-running.md).
