
Navio features three different receivers: NEO-7M in the standard Navio, NEO-6T in the Navio RAW version and NEO-M8N in Navio+. All of them are compatible with this tutorial. Full tech specs are available at the official [product page](http://www.u-blox.com/en/gps-modules/pvt-modules.html). These GPS modules are connected over SPI and send messages, containing location information and receive messages with configuration data.


####U-blox NEO example
This example is designed to show an easy way to capture and decode UBX protocol messages. For simplicity, it only parses UBX protocol NAV-POSLLH messages. NAV-POSLLH details, as well as full UBX protocol description can be seen [here](http://www.u-blox.com/images/downloads/Product_Docs/u-blox6_ReceiverDescriptionProtocolSpec_%28GPS.G6-SW-10018%29.pdf). The output of the example data is: current longitude and latitude, current height and the iTOW parameter. iTOW is the current millisecond time of week.


Now move to the folder Navio/Examples/GPS, compile and run the example.

```
cd Navio/C++/Examples/GPS
make gps
./gps
```

After you run the code, you will start seeing messages with current location data. Note that it takes some time for the receiver to get it's position and at first you will see zero value of latitude, longitude and height. iTOW parameter will change every second. This example starts an infinite loop, so when you are done, just stop the process with **CTRL+C**.

####U-blox NEO driver
Now, let's look through the example code:

```C++
#include "Navio/Ublox.h" 

using namespace std;

int main(int argc, char *argv[]){

    // This vector is used to store location data, decoded from ubx messages.
    // After you decode at least one message successfully, the information is stored in vector 
    // in a way described in function decodeMessage(std::vector&amp; data) of class UBXParser

    std::vector pos_data;

    // create ublox class instance
    Ublox gps;

    // testConnection() waits for a ubx protocol message and checks it.
    // If there's at least one correct message in the first 300 symbols the test is passed
    if(gps.testConnection())
    {
        printf("Ublox test OKn");

        // gps.decodeMessages();
        // You can use this function to decode all messages, incoming from the GPS receiver.
        // The function starts an infinite loop. 
        // In this example we can only decode NAV-POSLLH messages, the others are simply ignored. 
        // You can add new message types in function decodeMessage() of class UBXParser

        // Here, however we use a different approach. Instead of trying to extract info 
        // from every message(as done in decodeMessages()),
        // this function waits for a message of a specified type 
        // and gets you just the information you need. 
        // In this example we decode NAV-POSLLH messages, adding new types, however, is easy. 

        while (true) 
        {
            if (gps.decodeSingleMessage(Ublox::NAV_POSLLH, pos_data) == 1)
            {
                // after desired message is successfully decoded, we can use the information
                // stored in pos_data vector right here, or we can do something with it 
                // from inside decodeSingleMessage() function(see Ublox.h).
                // the way, data is stored in pos_data vector is specified in decodeMessage()
                // of class UBXParser(see Ublox.h)
                printf("Current location data:n");
                printf("iTOW: %lfn", pos_data[0]/1000);
                printf("Latitude: %lfn", pos_data[2]/10000000);
                printf("Longitude: %lfn", pos_data[1]/10000000);
                printf("Height: %lfn", pos_data[3]/100);

            } else {
                // printf("Message not capturedn");
                // use this to see, how often you get the right messages
                // to increase the frequency you can turn off the undesired messages
                // or tweak ublox settings to increase internal receiver frequency
            }
            usleep(200);
        }

    } else {
        printf("Ublox test not passednAbort program!n");
    }

    return 0;
}
```

SPI communication is defined in the SPI class, located at `Navio/Navio/SPIdev.h`. This code uses a Ublox class, which can be found in `Navio/Navio/Ublox.h`.Ublox class features a separate
scanner and parser, to take care of the incoming Ublox data.
If you want to decode a different type of message, you can add it to the function **decodeMessage()** of class UBXParser, just like NAV-POSLLH is defined there:

```
switch(id){
    case 258: {
            // ID for Nav-Posllh messages is 0x0102 == 258
            // In this example we extract 4 variables - longitude, latitude, 
            // height above ellipsoid and iTOW - GPS Millisecond Time of Week

            // All the needed parameters are 4-byte numbers with little endianness. 
            // We know the current message and we want to update the info in the data vector.
            // First we clear the old data:

            data.clear();

            // Second, we extract the data from the message buffer and save it to the vector.

            //iTOW
            data.push_back ((*(message+pos+9) &lt;&lt; 24) | (*(message+pos+8) &lt;&lt; 16) | (*(message+pos+7) &lt;&lt; 8) | (*(message+pos+6)));
            //Longitude
            data.push_back ((*(message+pos+13) &lt;&lt; 24) | (*(message+pos+12) &lt;&lt; 16) | (*(message+pos+11) &lt;&lt; 8) | (*(message+pos+10)));
            //Latitude
            data.push_back ((*(message+pos+17) &lt;&lt; 24) | (*(message+pos+16) &lt;&lt; 16) | (*(message+pos+15) &lt;&lt; 8) | (*(message+pos+14)));
            //Height
            data.push_back ((*(message+pos+21) &lt;&lt; 24) | (*(message+pos+20) &lt;&lt; 16) | (*(message+pos+19) &lt;&lt; 8) | (*(message+pos+18)));
        }   

        break;

```
Note, that to enable a certain type of message in the receiver, you need to send a configuration message first. For advanced configuration, you can use 
[U-center](Navio-dev/GPS-ucenter/) software.