####Currently supported boards

APM autopilot port for Navio+ currently works with:

* Raspberry Pi Model A
* Raspberry Pi Model B
* Raspberry Pi Model A+
* Raspberry Pi Model B+
* Raspberry Pi 2 Model B

Ports in progress, but not complete:

* Odroid-C1 (GPS and RCInput currently not working)

It is completely safe to use Navio+ with all boards stated above.

####Attaching Navio+ to a Raspberry Pi

* Install spacers to the top side of Raspberry Pi and fix them with screws from the bottom.
* Connect extension header to the 40-pin gpio port.
* Attach Navio+ to the extension header.
* Fix Navio+ using screws.

####Powering Navio+

Navio+ has three power sources, all of them can be used simultaneously as they are protected by ideal diodes. 

**IMPORTANT: ALL POWER SOURCES SHOULD PROVIDE VOLTAGE IN 4.8-5.3V RANGE, OTHERWISE YOU CAN DAMAGE YOUR NAVIO+ AND RASPBERRY PI.**

***For testing and development purposes:***

Connect 5V 1A power adapter to the Raspberry Pi’s microUSB port. Raspberry Pi will provide power to the Navio+.

***In a drone:***

Navio+ should be powered by a power module connected to the “POWER” port on Navio+. Navio+ will provide power to the Raspberry Pi.
![power-module](Navio-APM/img/NavioPlus-PowerModule.png)


***Redundancy:***

In case of power module failure Navio+ will switch to power from the servo rail.

####Powering servo rail

Power module does not power servos. To provide power to the servo rail plug your drone’s BEC into any free channel on the servo rail. Use BECs that provide voltage in a range of 4.8-5.3V. If you’d like to use high voltage servos, use a power separation board.

####GNSS antenna
![antenna](Navio-APM/img/NavioPlus-GNSSantenna.png)

####RC input
Navio+ is a PPM only autopilot. That means that you will have to use receiver with PPM output, PPM decoder or SBUS to PPM converter. Here is a list of receivers with PPM output:

**For ACCST (most FrSky transmitters):**

* FrSky D4R-II 4ch 2.4Ghz ACCST Receiver
* FrSKY V8R7-SP ACCST 7 Channel RX with composite PPM
* FrSKY D8R-XP

**For FASST (Futaba & some FrSky trasmitters):**

FrSky TFR4 4ch 2.4Ghz Surface/Air Receiver FASST Compatible

![rcin](Navio-APM/img/NavioPlus-RCInput.png)
####RC output

Power module does not provide power to servos so a BEC should be present. BEC would serve as back-up power supply to Navio+.

![escservos](Navio-APM/img/NavioPlus-RCOutputESCandServos.png)

Only one ESC central wire should be connected to Navio+ otherwise BECs built in ESCs will heat each other.

![esc](Navio-APM/img/NavioPlus-RCOutputESCs.png)

#### Telemetry modem

Radio modems can be connected either over UART or over USB. 

For UART port use /dev/ttyAMA0 serial. 
Please do not connect CTS line when using 3DR Radio as RPi does not handle hardware flow control properly.
![uartradio](Navio-APM/img/NavioPlus-UARTradiomodem.png)

Use /dev/ttyUSB0 virtual serial port for USB.
![usbradio](Navio-APM/img/NavioPlus-USBradiomodem.png)

#### Barometer UV protection

MS5611 barometer (steel cap IC) is sensitive to UV light and might report sudden jumps in altitude under sunlight. It is very important to cover it with a piece of cloth (something like microphone fabric) or put autopilot in a protective case to protect it both from sunlight and airstreams. 
