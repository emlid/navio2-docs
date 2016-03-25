**RGB LED**

If you haven't already done that, download Navio2 drivers and examples code like this:
```bash
git clone https://github.com/emlid/Navio2.git
```  

***C++***

Move to the folder with the source code, compile and run the example
```bash
cd Navio/C++/Examples/LED2
make
sudo ./LED
```
***Python***

Move to the folder with the code and run the example:
```bash
cd Navio2/Python
sudo python LED.py
```  

In this example Navio2 LED  changes color every second and outputs the color name to console.
```bash
LED is yellow
LED is green
LED is cyan
LED is blue
```

For further information see source code. You can change LED color using ```led.setColor()``` function, which takes color name as an argument. List of colors defined in ```Navio2/C++/Navio/RGBled.h```
