![](http://www.emlid.com/wp-content/uploads/2014/10/ArduPilot.png)

#### Where to get the code

Navio2 is supported in the main [ArduPilot repository](https://github.com/ArduPilot/ardupilot).

#### How to build

ArduPilot binary for Navio2 can be built using two ways:

1) Directly on your Raspberry Pi. Simpler, but slower. Build takes approximately 15 minutes.

2) Using a cross-compiler (on Linux PC or virtual machine). This is much faster, but requires one-time setup.

If you'd like to build on Raspberry Pi or use Waf build system (either on Raspberry Pi or Linux PC) skip the next step.

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

#### Building ArduPilot using make-based build system

<sub> **Deprecated**. Please use [waf](building-from-sources/#building-apm-using-waf-build-system) to build the project. </sub>

#### Building ArduPilot using Waf build system

These steps are the same both for compiling ArduPilot directly on Raspberry Pi and cross-compiling.

Download the ArduPilot code and update submodules:

```bash
git clone https://github.com/ArduPilot/ardupilot.git
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

Now you can build arducopter. For quadcopter use the following command:
```bash
waf --targets bin/arducopter-quad
```  
To build for other frame types replace quad with one of the following options:
```bash
coax heli hexa octa octa-quad single tri y6
```
In the end of compilation binary file with the name ```arducopter-quad``` will be placed in ```ardupilot/build/navio2/bin/``` directory.

If you're using cross-compiler transfer the binary to your Raspberry Pi:

```bash
rsync -avz ardupilot/build/navio2/bin/arducopter-quad pi@192.168.1.3:/home/pi/
```

Where 192.168.1.3 is an IP address of your Raspberry Pi with Navio2.

For further information read [ardupilot WAF Build](https://github.com/ArduPilot/ardupilot/blob/master/BUILD.md).


Instructions how to run ArduPilot on Raspberry Pi and to connect GCS to it are available in  [Installation and running section](installation-and-running.md).
