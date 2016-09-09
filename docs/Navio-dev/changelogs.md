##Navio Raspbian changelog

*2016-07-18:*

- Added driver for rtl8812-based Wi-Fi dongles
- Moved image snapshot closer to upstream
- Updated rcio-dkms to 0.6

##Navio ArduPilot changelog

*3.4-rc4:*

- Miscellaneous EKF improvements
- New thread per bus API
- MS5611 can never block the main thread as all bus operations are executed on another thread using the new API
- Guided_NoGPS flight mode added to allow simpler control in non-GPS environments

*3.4-rc1:*

- LSM9DS1 support added with correct orientation set as default
- Several compasses work out of the box
- Various EKF improvements
- Different frequency on AUX PWM channels
- 16 RC Input channels are supported
- ADC channels work as expected

##RCIO kernel module

*0.6.1:*

- fixed a potential alt/default PWM rate misconfiguration that could lead to problems on octo/octo-quads
