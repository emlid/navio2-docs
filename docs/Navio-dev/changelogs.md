##Navio Raspbian changelog

*2017-03-11:*

- fix Beta v4's upgrade issue:

This happened due to a fact that some of the packages are implicitely dependent on libraspberrypi0

- make Emlid distribution fully compliant with official Raspbian

This means that all packages that depend on libraspberrypi0 and raspberrypi-bootloader will work as expected.
For example ```sudo apt-get install python-picamera``` will work

- add python3, python3-pip, mavproxy, droneapi, tmux
- emlitool: 0.8.4

- ```apt-mark hold  ros-indigo-mavros``` to discourage updates.

Indigo depends on 0.17.x versions of mavros which doesn't handle properly some Boost error handling. Hence some monkey patching is needed.

- fix rcio-dkms updates:

rcio.dtbo could get not regenerated for newer kernels. The latter is now part of ```raspberrypi-kernel-emlid``` package.

- update ArduPilot-related stuff:
    Arducopter: 3.4.5
    ArduPlane: 3.7.1
    APMrover2: 3.1.0
    ardupilot.service: 0.9.0


*2017-01-19:*

- fixed /boot/wpa_supplicant.conf issue that made it dissapear after the first boot

*2016-12-29:*

- ArduPilot with ArduCopter 3.4.4-rc1 is the new default
- Wi-Fi broadcast support
- Misc minor bugfixes

*2016-12-01:*

- ArduPilot with ArduCopter 3.4.3-rc1 is the new default
- Various minor ad-hoc fixes

*2016-10-31:*

- Added ROS and mavros support
- Included navio-config utility
- ArduPilot with ArduCopter 3.4.0 preinstalled by default
- Added new start-up scripts for ArduPilot to ease launching on boot
- Versioning in /etc/rpi-issue
- Miscellaneous minor fixes


*2016-07-18:*

- Added driver for rtl8812-based Wi-Fi dongles
- Moved image snapshot closer to upstream
- Updated rcio-dkms to 0.6

##Navio ArduPilot changelog

#ArduCopter

*3.4.4*

- Minor EKF updates

*3.4.4-rc1*

- Miscalleneous EKF fixes

*3.4.3-rc1*

- LSM9DS1 magnetometer is the new default rendering 'Inconsistent Compasses' error obsolete
- AK8963 is disabled by default
- Power module definitions are updated to ease configuration

*3.4.0*

- First stable release with support for Navio 2
- EKF2 is used by default

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

#ardupilot.service

*0.9.0*

- Include all supported frames

##RCIO kernel module

*0.6.4:*

- fixed erratic PWM output on first 8 channels

*0.6.1:*

- fixed a potential alt/default PWM rate misconfiguration that could lead to problems on octo/octo-quads

## emlitool

*0.8.4*

- Initial release with board information and sensor testing one-click utility
