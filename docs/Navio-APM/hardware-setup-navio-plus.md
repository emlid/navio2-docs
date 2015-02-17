####GNSS antenna
![antenna](Navio-APM/img/Navio+GNSSantenna.png)
####RC input
Navio+ is a PPM only autopilot. That means that you will have to use receiver with PPM output, PPM decoder or SBUS to PPM converter. Here is a list of receivers with PPM output:

**For ACCST (most FrSky transmitters):**

* FrSky D4R-II 4ch 2.4Ghz ACCST Receiver
* FrSKY V8R7-SP ACCST 7 Channel RX with composite PPM
* FrSKY D8R-XP

**For FASST (Futaba & some FrSky trasmitters):**

FrSky TFR4 4ch 2.4Ghz Surface/Air Receiver FASST Compatible

![rcin](Navio-APM/img/Navio+RCInput.png)
####RC output

Power module does not provide power to servos so a BEC should be present. BEC would serve as back-up power supply to Navio+.

![escservos](Navio-APM/img/Navio+RCOutputESCandServos.png)

Only one ESC central wire should be connected to Navio+ otherwise BECs built in ESCs will heat each other.

![esc](Navio-APM/img/Navio+RCOutputESCs.png)
#### Telemetry modem
![uartradio](Navio-APM/img/Navio+UARTradiomodem.png)

![usbradio](Navio-APM/img/Navio+USBradiomodem.png)
