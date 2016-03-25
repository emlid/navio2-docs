![3DIMU](http://www.emlid.com/wp-content/uploads/2014/10/3DIMU.png)

One of examples for Navio2 demonstrates the work of Mahony AHRS with the data from an onboard MPU9250 or LSM9DS1 sensor. We’ve also made a simple but cool visualizer for it that you can run on your PC\Mac. Here’s the instruction how to run AHRS and visualizer:

#### Preparing your Mac

Install [pip](https://pip.pypa.io/en/latest/installing.html) and use it to get required packages:

```bash
sudo pip install PyOpenGL PyOpenGL_accelerate
sudo pip install pyserial
```

You might be asked to install command line developer tools along the way.

#### Preparing your PC

Install OpenGL
```bash
sudo apt-get install python-opengl
```

#### On PC\Mac

Download [the archive with Navio utilities](https://github.com/emlid/Navio2/archive/master.zip)
Extract the archive, enter the directory with 3DIMU utility and run it:

```bash
cd Navio2/Utilities/3DIMU
sudo python 3Dimu.py
```
#### On Raspberry Pi

Clone Navio2 repository, navigate to the folder with AHRS example, compile and run it:

```bash
git clone https://github.com/emlid/Navio2.git
cd Navio2/C++/Examples/AHRS
make
sudo ./AHRS -i [sensor name] X.X.X.X 7000
```

Where X.X.X.X is an ip address of your PC. 7000 is the port number used in 3DIMU. Argument [sensor name] allows you to choose inertial measurement unit: mpu is MPU9250, lsm is LSM9DS1.

Now try to move your Navio2 and check how the “brick” on the screen moves along with it.
