#####Setting up Raspbian
We have an image of Raspbian with all the required configurations available for download. It features real-time kernel with lower latency than default kernel. You can get it in the downloads section. If you want to use default Raspbian then some configuration is necessary - SPI and I2C have to be enabled.

SSH to Raspberry Pi and edit the configuration file:

```bash
sudo nano /etc/modprobe.d/raspi-blacklist.conf
```

This will open a text editor, now change

```bash
blacklist spi-bcm2708
blacklist i2c-bcm2708
```
to

```bash
#blacklist spi-bcm2708
#blacklist i2c-bcm2708
```

Then press CTRL-C and answer Yes to save the file.

To set SPI and I2C modules to load on boot edit /etc/modules:

```bash
sudo nano /etc/modules
```
Add these strings if not present:

```bash
i2c-bcm2708
spi-dev
i2c-dev
```
Save the file and reboot.

After the reboot check that /dev/spidev0.0, /dev/spidev0.1 and /dec/i2c-1 are present in the system.
#####Detecting I2C devices with i2cdetect tool
Tool called i2cdetect allows you to see devices connected to the I2C bus. It's avaiable in the package i2c-tools, to install it run:

```bash
sudo apt-get install i2c-tools
```
Run it like this:

```bash
sudo i2cdetect -y 1
```
Where 1 is the number of I2C bus.
The result should look like this:

```bash
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: 40 -- -- -- -- -- -- -- 48 -- -- -- -- -- -- --
50: 50 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: 70 -- -- -- -- -- -- 77
```
You should see all devices that responded to their addresses.
####Addresses of onboard devices
I2C devices:

* ADS1115 - 0x48
* MS5611 - 0x77
* MB85R - 0x50
* PCA9685 - 0x40, 0x70 (allcall address)

SPI devices:

* U-blox NEO - /dev/spidev0.0
* MPU9250 - /dev/spidev0.1

#####Using UART port
By default UART on Raspberry Pi is used for serial console. To use UART for custom data serial console has to be disabled. It can be done by using the raspi-config utility:

```bash
sudo raspi-config
```
Open "Advanced options" -> "Serial console" and disable it. 
Now you can use /dev/ttyAMA0 as a UART.
