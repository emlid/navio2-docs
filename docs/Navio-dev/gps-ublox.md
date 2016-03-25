
Navio features three different receivers: NEO-7M in the standard Navio, NEO-6T in the Navio RAW version and NEO-M8N in Navio+ and Navio2. All of them are compatible with this tutorial. Full tech specs are available at the official [product page](http://www.u-blox.com/en/gps-modules/pvt-modules.html). These GPS modules are connected over SPI and send messages, containing location information and receive messages with configuration data.

**U-blox NEO example**

This example is designed to show an easy way to capture and decode UBX protocol messages. For simplicity, it only parses UBX protocol NAV-STATUS and NAV-POSLLH messages. NAV-POSLLH details, as well as full UBX protocol description can be seen [here](http://www.u-blox.com/images/downloads/Product_Docs/u-blox6_ReceiverDescriptionProtocolSpec_%28GPS.G6-SW-10018%29.pdf). The output of the example data is: current GPS status, current longitude and latitude, current height above Ellipsoid, current height above mean sea level, vertical and horizontal accuracy estimate and the iTOW parameter. iTOW is the current millisecond time of week.

If you haven't already done that, download Navio2 drivers and examples code like this:
```bash
git clone https://github.com/emlid/Navio2.git
```

***C++***  
Move to the folder Navio2/C++/Examples/GPS, compile and run the example.
```bash
cd Navio2/C++/Examples/GPS
make
./gps
```

***Python***  
Move to the folder with the source code and run the example:
```bash
cd Navio2/Python
python GPS.py
```  

After you run the code, you will start seeing messages with current location data. Note that it takes some time for the receiver to get it's position and at first you will see zero value of latitude, longitude and height. iTOW parameter will change every second. This example starts an infinite loop, so when you are done, just stop the process with **CTRL+C**.


For further information see source code. SPI communication is defined in the SPI class, located at `Navio2/C++/Navio/SPIdev.h`. This code uses a Ublox class, which can be found in `Navio2/C++/Navio/Ublox.h`.Ublox class features a separate scanner and parser, to take care of the incoming Ublox data.
If you want to decode a different type of message, you can add it to the function **decodeMessage()** of class UBXParser.

Note, that to enable a certain type of message in the receiver, you need to send a configuration message first. For advanced configuration, you can use
[U-center](gps-ublox-ucenter/) software.

More information about the GPS receiver is available in [U-blox NEO-M8 datasheet](https://www.u-blox.com/sites/default/files/NEO-M8_DataSheet_%28UBX-13003366%29.pdf).

Information about Navio GNSS antenna is available in [ANT101 - GPSGLONASS PCB Active Antenna](http://files.emlid.com/ANT101-PCB-Antenna-Datasheet.pdf).
