####Mounting Navio on Raspberry Pi

Navio was designed to fit on top of Raspberry Pi Model A and Model B, however it also works with Raspberry Pi Model B+ due to the backward compatibility between them.  Mounting hole is 2.9mm to accommodate M2.5 screw or bolt.

####Powering Navio and Raspberry Pi

***From the single power source over USB***

This method could be used for working with sensor data only. Raspberry Pi powers the Navio board but no voltage on the servo rail is present. 5VRPI pin on header corresponds to Raspberry Pi 5V network and is directly connected to USB source. Never connect two power sources to this network at the same time, it will damage the power supply. If you just want to do PPM decoding or power something over the servo rail you can add a jumper between 5VRPI and BEC. This way the whole servo rail is powered from the Pi, please do not connect servos in this configгration, it will cause voltage spikes and Raspberry Pi will reboot. Use power adapters that can handle at least 1A of current.

***From separate power sources over 5VRPI and BEC power inputs on servo header***

In case you want to control servos, motors or other high power loads it is necessary to use separate power sources for Navio servo rail and Raspberry Pi since the latter is very sensitive to voltage dropouts. Use two BECs or another DC sources that provide 5V and connect them to 5VRPI and BEC inputs on Navio board.
5VRPI input and BEC input should be strictly 4.75-5.25V.

***Voltage levels on ports and headers***

Servo rail power pins – 5V
Servo rail PWM pins – 5V
PPM input pin – 5V
UART, SPI, I2C port power pins – 5V
UART, SPI, I2C signal pins – 3.3V (**CAUTION!** connecting 5V logic device to these pins may damage your Raspberry Pi!)
ADC power pin – 3.3V output
ADC signal pins – 0V-3.3V input



