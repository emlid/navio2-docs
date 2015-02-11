![circle](http://www.emlid.com/wp-content/uploads/2014/10/Navio-RTKLIB1.png)

RTKLIB is a set of open source programs for GNSS data processing written by Tomoji Takasu. It’s main function is software processing of raw GNSS data in differential mode allowing to achieve centimeter or decimeter accuracy depending on the hardware used. It’s an amazing software as it provides the possibility to do RTK without spending a few thousand dollars on equipment. Navio Raw is equipped with U-blox NEO-6T GPS receiver that provides raw data necessary for RTK processing in RTKLIB.


####Downloading RTKLIB
U-blox receiver on Navio is connected to Raspberry Pi over SPI, which is unusual (but necessary since interfaces are limited on RPi) as receivers are mostly connected over UART and thus not all software supports SPI. We’ve forked RTKLIB and added SPI support for input stream.  You can download it from your Raspberry Pi.

```
wget https://github.com/emlid/RTKLIB/archive/master.zip
unzip master
```

The archive contains ready to use RTKLIB binaries for Raspberry Pi:

```
cd RTKLIB-master/bin-rpi
```

####Running RTKLIB
RTKLIB’s real-time processing program is RTKRCV, to run it type:

```
./rtkrcv

```
**Setting up Navio Raw as a single mode rover**

Let’s start with single mode first and then reconfigure RTKLIB for differential mode. Single mode works with one GPS receiver without differential corrections and implements a usual GPS receiver. We’ve prepared the config for Navio that sets up RTKLIB to work with onboard U-blox receiver and calculate coordinates in a single mode. To load it type in RTKRCV’s console:

```
load rover-single.conf
```

After that you can start the processing:
```
start
```
To view status including number of received observations and coordinates:
```
status
```
To enable regular status updates over time add period value to the status command:
```
status 1
```
Here's and example of status output in single mode:

<a href="http://www.emlid.com/wp-content/uploads/2014/10/rtklib-single.png"><img class="alignnone size-full wp-image-2730" src="http://www.emlid.com/wp-content/uploads/2014/10/rtklib-single.png" alt="rtklib-single" width="1140" height="725" /></a>
Note the following parameters:
**# of input data rover obs() and nav()** - represents the number of messages from the receiver, obs value should be increasing by 5 every second, since the receiver is configured to output with that rate, nav value should get increased a couple of times per minute.
**pos llh single rover** - calculated lat-lon-height coordinate of the antenna.

To stop regular status updates press:
```
CTRL^C
```
To stop processing use the following command:
```
stop
```
To exit RTKLIB type:
```
shutdown
```
####Configuration options
Options can be set in the console of RTKRCV but it’s more useful to write them in configuration file. There are lots of options, but let’s examine those that we’ve changed from the default config (rtkrcv.conf):

```
inpstr1-type = spi // Connecting to U-blox receiver over SPI
inpstr1-path = /dev/spidev0.0 // SPI bus and chip select for U-blox receiver
inpstr1-format = ubx // Data is in u-blox’s binary format 
file-cmdfile1 =../data/ubx_spi_raw_5hz.cmd // Configure u-blox to output raw data on SPI
pos1-posmode = single // Working in single position mode
pos1-sateph = brdc // Using ephemeris from the broadcast (precise is default)
```

####Setting up Navio Raw as an RTK rover
In order to configure RTKLIB to process differential corrections it is necessary to specify some additional options:

```
inpstr2-type = What type of connection is used to get base station data
inpstr2-path = Path to base station data stream, e.g. ipaddress:port for TCP
inpstr2-format = Data format for base station stream, the most usual is RTCM
pos1-posmode = Use ‘static’ for non-moving rovers and ‘kinematic’ for moving
```


If coordinates of the base station are available in the RTCM data stream then specify:
```
ant2-postype = rtcm
```

If you’re not using RTCM that transfers base station coordinates then you may need to specify it’s coordinates yourself in the following options:
```
ant2-postype = llh
ant2-pos1 = (latitude)
ant2-pos2 = (longitude)
ant2-pos3 = (height)
```

After that you can restart RTKRCV server and check the status. Pay attention to the solution status, it can be:
* Single - Only the rover data is available, no corrections from the base.
* Floating - Corrections from the base are available, but it either cannot see enough satellites with proper SNR to for accurate calculation, or there are not enough satellites in common with the base station for valid correction.
* Fixed - There are at least five satellites common for rover and the base and the solution is
calculated correctly.

Here’s an example of status output of RTKRCV running in static mode. Output was taken when rover’s antenna was located inside the building near the window for demonstration purposes only.
![rtklib-static](http://www.emlid.com/wp-content/uploads/2014/10/rtklib-static.png)

####Output and logging options
RTKLIB can produce an output of calculated coordinates in a specified format. For example, it can output NMEA stream on a TCP port that you can use in other software to get accurate coordinates. The following options have to be specified:
```
outstr1-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr) 
outstr2-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr) 
outstr1-path = 
outstr2-path = 
outstr1-format =llh # (0:llh,1:xyz,2:enu,3:nmea) 
outstr2-format =nmea # (0:llh,1:xyz,2:enu,3:nmea)
```
Also, RTKLIB can log the incoming data for later analysis or post-processing. Following options specify the type and path of the log:
```
logstr1-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr) 
logstr2-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr) 
logstr3-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr) 
logstr1-path = 
logstr2-path = 
logstr3-path =
```

####Setting up Navio Raw as a base station
str2str utility can be used to turn Navio Raw in a base station. Just run it with a few options:

```
cd RTKLIB/bin-rpi
./str2str -in spi:///dev/spidev0.0:245000:0#ubx -out tcpsvr://:9000#rtcm3 -c ../data/ubx_spi_raw_5hz.cmd
```

Now you can specify the IP address of this Raspberry Pi with port 9000 as a base station in the config of your rover like this:

```
inpstr2-type = tcpcli
inpstr2-path = X.X.X.X:9000 #X.X.X.X is the IP address of your base station Raspberry Pi
inpstr2-format = rtcm3
```

You may also want to specify additional options:
```
-msg type[(tint)][,type[(tint)]...] rtcm message types and output intervals (s) 
-sta sta station id 
-opt opt receiver dependent options 
-s msec timeout time (ms) [10000] 
-r msec reconnect interval (ms) [10000] 
-f sec file swap margin (s) [30] 
-p lat lon hgt station position (latitude/longitude/height) (deg,m) 
```


####Requirements for achieving good results with RTK
* Use high quality antenna. They perform better than usual patch antennas due to the advanced technology. We recommend antennas from Tallysman with low noise and great performance, such as [TW2105](http://www.tallysman.com/TW2105.php)
* Use bigger ground plane. Place your antenna on a piece of metal. The size of ground plane is usually specified in antenna’s datasheet. Usual values are 100x100mm or 70x70mm.
* Check satellite visibility before going into the field. Online services like  [SatPredictor](http://satpredictor.navcomtech.com/) can show satellite visibility over time as well as elevations. You need at least five visible satellites that are common for rover and the base. Higher satellite elevation is better.
* Stay away from noise sources such as mobile phone stations.

####Real-time demonstration
Here's a video we made some time ago showing a rover moving in a circle on a dirt road.

<iframe width="560" height="315" src="https://www.youtube.com/embed/5mCUCnCX2oQ" frameborder="0" allowfullscreen></iframe>


####References
If you'd like to get more information about RTKLIB please refer to <a href="http://www.rtklib.com/prog/manual_2.4.2.pdf">RTKLIB Manual</a>.