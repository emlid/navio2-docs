
####MPU9250
MPU9250 is one of the best in class inertial sensors, which combines a gyroscope, an accelerometer and a magnetometer in one device. MPU sensor family is not only popular as a part of drone autopilot projects, but is also widely used in devices like cellphones, tablets, etc.

####IMU example
If you haven't already done that, download Navio drivers and examples code like this:

```bash
git clone https://github.com/emlid/Navio
```

Move to folder Navio/Examples/AccelGyroMag, compile and run the example

```
cd Navio/C++/Examples/AccelGyroMag
make 
./AccelGyroMag
```

You should immediately see 9 values, updated in real time. Try to move the device around and see them change. They include Accelerometer, Gyroscope and Magnetometer data, three axis each.
####MPU9250 driver
Let's see the example code:

```c
#include "Navio/MPU9250.h"

int main()
{
   MPU9250 imu;
   imu.initialize();

   float ax, ay, az, gx, gy, gz, mx, my, mz;

   while(1) {
      imu.getMotion9(&ax, &ay, &az, &gx, &gy, &gz, &mx, &my, &mz);
      
      printf("Acc: %+05.3f %+05.3f %+05.3f ", ax, ay, az);
      printf("Gyr: %+05.3f %+05.3f %+05.3f ", gx, gy, gz);
      printf("Mag: %+05.3f %+05.3f %+05.3fn", mx, my, mz);

      sleep(0.5);
   }
}
```

The first thing we should pay attention to is the line ***imu.initialize()***, as it does an important job of setting internal device parameters:

```c
bool MPU9250::initialize(int sample_rate_div, int low_pass_filter)
{
    uint8_t i = 0;
    uint8_t MPU_Init_Data[MPU_InitRegNum][2] = {
        //{0x80, MPUREG_PWR_MGMT_1},     // Reset Device - Disabled because it seems to corrupt initialisation of AK8963
        {0x01, MPUREG_PWR_MGMT_1},     // Clock Source
        {0x00, MPUREG_PWR_MGMT_2},     // Enable Acc &amp; Gyro
        {low_pass_filter, MPUREG_CONFIG},  // Use DLPF set Gyroscope bandwidth 184Hz, temperature bandwidth     188Hz
        {0x18, MPUREG_GYRO_CONFIG},    // +-2000dps
        {0x08, MPUREG_ACCEL_CONFIG},   // +-4G
        {0x09, MPUREG_ACCEL_CONFIG_2}, // Set Acc Data Rates, Enable Acc LPF , Bandwidth 184Hz
        {0x30, MPUREG_INT_PIN_CFG},    //
        //{0x40, MPUREG_I2C_MST_CTRL},   // I2C Speed 348 kHz
        //{0x20, MPUREG_USER_CTRL},      // Enable AUX
        {0x20, MPUREG_USER_CTRL},       // I2C Master mode
        {0x0D, MPUREG_I2C_MST_CTRL}, //  I2C configuration multi-master  IIC 400KHz
        {AK8963_I2C_ADDR, MPUREG_I2C_SLV0_ADDR},  //Set the I2C slave address of AK8963 and set for write.
        //{0x09, MPUREG_I2C_SLV4_CTRL},
        //{0x81, MPUREG_I2C_MST_DELAY_CTRL}, //Enable I2C delay
        {AK8963_CNTL2, MPUREG_I2C_SLV0_REG}, //I2C slave 0 register address from where to begin data transfer
        {0x01, MPUREG_I2C_SLV0_DO}, // Reset AK8963
        {0x81, MPUREG_I2C_SLV0_CTRL},  //Enable I2C and set 1 byte
        {AK8963_CNTL1, MPUREG_I2C_SLV0_REG}, //I2C slave 0 register address from where to begin data transfer
        {0x12, MPUREG_I2C_SLV0_DO}, // Register value to continuous measurement in 16bit
        {0x81, MPUREG_I2C_SLV0_CTRL}  //Enable I2C and set 1 byte
    };

for(i=0; i &lt; MPU_InitRegNum; i++) {
WriteReg(MPU_Init_Data[i][1], MPU_Init_Data[i][0]);
usleep(100000); //I2C must slow down the write speed, otherwise it would not work
}

set_acc_scale(BITS_FS_16G);
set_gyro_scale(BITS_FS_2000DPS);

calib_mag();
return 0;
}

```

Note that this function also sets scales for both Accelerometer and Gyroscope:

```c
{0x18, MPUREG_GYRO_CONFIG},    // +-2000dps
{0x08, MPUREG_ACCEL_CONFIG},   // +-4G
```

The main function loop is pretty straightforward: read the data, print the data.

You can find additional information about MPU9250 in the datasheet.
