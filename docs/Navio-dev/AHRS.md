![3DIMU](http://www.emlid.com/wp-content/uploads/2014/10/3DIMU.png)


One of examples for Navio demonstrates the work of Mahony AHRS with the data from an onboard MPU9250 sensor. We’ve also made a simple but cool visualizer for it that you can run on your PC\Mac. Here’s the instruction how to run AHRS and visualizer:
####Preparing your Mac
Install [pip](https://pip.pypa.io/en/latest/installing.html) and use it to get required packages:

```
sudo pip install PyOpenGL PyOpenGL_accelerate
sudo pip install pyserial
```

You might be asked to install command line developer tools along the way.
####On PC\Mac
Download [the archive with Navio utilities](https://github.com/emlid/Navio/archive/master.zip)
Extract the archive, enter the directory with 3DIMU utility and run it:

```
cd Navio/Utilities/3DIMU
python 3Dimu.py
```
####On Raspberry Pi
Clone Navio repository, navigate to the folder with AHRS example, compile and run it:

```
git clone https://github.com/emlid/Navio.git
cd Navio/C++/Examples/AHRS
make
./AHRS X.X.X.X 7000
```

Where X.X.X.X is an ip address of your PC. 7000 is the port number used in 3DIMU.

Now try to move your Navio and check how the “brick” on the screen moves along with it.