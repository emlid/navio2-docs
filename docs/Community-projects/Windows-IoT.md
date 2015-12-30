#Windows IoT

----------

**Disclaimer**

The Windows IoT SDK is a personal project of Emlid community member [Tony Wall](http://community.emlid.com/users/codechief).

For any support please refer to him in the [project thread](http://community.emlid.com/t/windows-10-iot-image-for-navio/381/last).

All following text is provided by the project author.

**End of disclaimer**

----------

##Project

In 2015 Microsoft announced official support for the Raspberry Pi 2 running Windows 10 on their new "IoT" (Internet of Things) platform. Soon after work started on an open source SDK for Navio with the following goals:

 - Enable access to all the great sensors of the Navio HAT for Windows DIY projects.
 - Prove the concept of an autopilot running a Windows OS (quadcopter in stabilized flight mode).
 - Provide the tools necessary for the community to extend existing autopilots to Windows and/or create an entirely new system.

This document describes develop your own code for Navio running on Windows 10 IoT, or just try-out the test and sample apps for yourself. You can follow progress in the [forum thread](http://community.emlid.com/t/windows-10-iot-image-for-navio/381/last) or the [Hackster.io project](https://www.hackster.io/team-code-for-robots/navio-sdk-for-windows-iot) sites. If you like it please "respect" the Hackster project to return some kudos to the creator ;-)

##Getting Started

This section describes how to start development by installing Windows IoT on your device, Visual Studio on your PC, then finally how to clone, build and run the code.

###Tools

Links to download the development tools and basic instructions what to install are provided by Microsoft here:

[Getting Started with Windows IoT](http://ms-iot.github.io/content/en-US/GetStarted.htm)

The "Windows 10 IoT Core Dashboard" tool eases the process of downloading the OS image to the Raspberry Pi 2, then helps you connect and configure devices over the network. End-users need only install the dashboard, or you could provide them with a preloaded SD card.

Alternatively you can download the ISO image and release notes here:

[Other Downloads](http://ms-iot.github.io/content/en-US/Downloads.htm)

Since device driver source code has been added to the solution, you will also need to install the latest Windows Driver Kit (WDK) available in the "Install WDK 10" link on this page:

[Driver Kit Downloads](https://msdn.microsoft.com/en-US/windows/hardware/dn913721%28v=vs.8.5%29.aspx?f=255&MSPPError=-2147217396)

##Development

The current Navio SDK "framework" is a Windows Universal library, so can be consumed by any Windows Universal application written in any .NET language supported by IoT. Simply use the NuGet Package Manager of Visual Studio to add the "Emlid.WindowsIot.Hardware" package, then write some code with the various Navio*Device classes to utilize the hardware.

Deployment is easy with the Visual Studio produced Windows Universal packages. They are precompiled and load directly onto the IoT device. You can deploy them via Visual Studio during development, via the web interface for end users, script them with PowerShell or include them on a preloaded SD card image.

###Building the SDK from Source

 1. Start Visual Studio 2015 then [follow the instructions](http://ms-iot.github.io/content/en-US/win10/samples/HelloWorld.htm) to build and deploy the "Hello World" sample. Now you know enough to build and deploy the Navio SDK!
 2. Update the GitHub extension (menu "Tools - Extensions and Updates...").
 3. Consider installing the "Productivity Power Tools 2015" for easy access to the command prompt (from the same "Extensions and Updates..." dialog).
 4. Click "Team Explorer" then the "Plug" icon (manage connections).
 5. Under "Local Git Repositories" click the "Clone" link then enter the URL of the GitHub project "[https://github.com/emlid/Navio-SDK-Windows-IoT](https://github.com/emlid/Navio-SDK-Windows-IoT)", choose a local path into which the files will be downloaded then the "Clone" button.
 6. Open the solution by selecting the "File - Open - Project/Solution..." menu option then the solution (.sln) file from the local repository directory you just cloned.
 7. On the build toolbar, ensure that "Debug" configuration and "ARM" platform is selected.
 8. In the "Build" menu select "Rebuild Solution". 
 9. Some NuGet packages may be loaded, then the build will start and should complete without any errors.

###Build and Run the Hardware Test App

The Hardware Test app is currently only distributed as source. Later it will also be available for stand-alone use (without any development tools).

 1. Ensure the Remote Debugging Tools are running on the IoT device. Use the web interface (easily accessible via the dashboard tool) "Debugging" page to start it and also check the remote name and port, which must match in your deployment configuration.
 2. On the build toolbar, ensure the "ARM" processor is selected and the "Navio Hardware Test" startup project.
 3. Click the drop-down arrow next to the green "play" icon, then "Remote Machine".
 4. If this is the first time you deployed to a remote machine the configuration dialog will appear, then you should enter the "name:port" of the target device and choose no authentication.
 5. With the remote machine still selected, click the "Play" icon to build, deploy and run the sample.
 6. The solution should compile, deploy and run on the Navio/RasPi. Use the mouse to select "LED & PWM" (the rest is not implemented yet). Check the frequency, set some sliders (best PWM near bottom) then click the Output ON.
 7. Play with the sliders to test your LED and servos/ESCs.
 8. Monitor the output window for any messages, and look at the physical device for any feedback, e.g. LED colours or servo movement.
 9. Click "Close" then "Exit" to finish.

###Build and Run the Samples

Currently the sample applications are "Background Application" projects, which means they run directly from Visual Studio with no UI. All output is sent back via debug messages to the Visual Studio "Output" window.

 1. Ensure the Remote Debugging Tools are running on the IoT device. Use the web interface (easily accessible via the dashboard tool) "Debugging" page to start it and also check the remote name and port, which must match in your deployment configuration.
 2. On the build toolbar, ensure the "ARM" processor is selected and the "Navio Hardware Test" startup project.
 3. Click the drop-down arrow next to the green "play" icon, then "Remote Machine".
 4. If this is the first time you deployed to a remote machine the configuration dialog will appear, then you should enter the "name:port" of the target device and choose no authentication.
 5. With the remote machine still selected, click the "Play" icon to build, deploy and run the sample.

Monitor the output window for any messages, and look at the physical device for any feedback, e.g. LED colours or servo movement.

##Roadmap

The current framework release is known as the "user mode" framework. Later it will be converted into a Windows Runtime Component so it it can be consumed by other languages than .NET, such as native C++ and NodeJS. The development language will also switch to lower-level C++ runtime components which will work in tandem with new Navio specific device drivers for performance and reliability. Once device drivers are available, a custom IoT image will also be produced to ease installation. OEM build instructions will also be included so you can integrate the individual drivers into your own IoT image.

The framework will remain, but the Navio*Device classes will be redirected to call the new lower level components. However it is inevitable that some breaking changes will occur. At least the user mode framework will continue to run on standard IoT images (without drivers installed) so you have to possibility to stay with the older NuGet package versions and upgrade when it suits you.

It remains to be seen what Microsoft will provide for provisioning and deployment of end user applications, and if there will be any other editions than "core". There could even be Windows Store support, some other kind of one-click installation. We can already provide manual download links to "side-load" standard packages onto the device via the web interface. The goal is to achieve "plug and play" installation and configuration of Navio and Navio enabled apps running Windows IoT.
