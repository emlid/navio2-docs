#Windows IoT Development

**Disclaimer**

SoftOSD is a personal project of Emlid community member Tony Wall.

For any support please refer to him in the [project thread](http://community.emlid.com/t/windows-10-iot-image-for-navio/381).

All following text in provided by the project author.

**End of disclaimer**

This page describes how to start development by installing Windows IoT on your device, Visual Studio on your PC, then finally how to clone, build and run the code.

##Prerequisites

Use the links on the at the end of the page to download the current Windows IoT Core image and Visual Studio 2015 Community Edition with Tools.

##Build and Run the Hardware Test App

1. Start Visual Studio 2015 then [follow the instructions](http://ms-iot.github.io/content/en-US/win10/samples/HelloWorld.htm) to build and deploy the "Hello World" sample. Now you know enough to build and deploy the Navio SDK!
2. Update the GitHub extension (Tools - Extensions and Updates...).
3. Clone [the GitHub project](https://github.com/emlid/Navio-SDK-Windows-IoT) in Visual Studio.
4. Open the solution (.sln) file in the root.
5. Ensure processor is ARM, select the "Navio Hardware Test" project then the drop-down arrow next to Debug then "Remote Machine".
6. The solution should compile, deploy and run on the Navio/RasPi. Use the mouse to select "LED & PWM" (the rest is not implemented yet). Check the frequency, set some sliders (best PWM near bottom) then click the Output ON.
7. Play with the sliders to test your LED and servos/ESCs.
8. Click Close then Exit to finish.

##Build and Run the Samples

Currently the sample applications are "Background Application" projects, which means they run directly from Visual Studio and all output is sent back via debug messages to the Output Window. This may be changed in the future to a "Command Line" or "Universal Application" project, so they can be executed remotely via PowerShell, SSH or the IoT web interface. This is probably the most straightforward method, at least until the way to package and deploy pre-built samples has be finalized.

1. On the build and run toolbar, change the start-up project to the sample app, e.g. the LED sample.
2. With the remote machine still selected, click Debug to build, deploy and run the sample.
3. Monitor the output window for any messages, and look at the physical device for any feedback, e.g. LED colours or servo movement.

#Windows IoT Downloads

Microsoft provide the master image of "Windows 10 IoT Core" for the Raspberry Pi 2 here:

[Windows IoT Download Page for Raspberry Pi 2](http://ms-iot.github.io/content/en-US/win10/SetupRPI.htm)

For the rest of the software, later you will be able to download either a pre-prepared image including the drivers and apps, or add the software to an existing installation. Although Windows 10 is "finished" the IoT embedded environment is still in "preview" stage.

It remains to be seen what Microsoft will provide for provisioning and deployment of end user applications, and if there will be any other editions than "core". It's very likely to have a Windows Store style one-click installation, or at least we will be able to provide manual download links which you can easily "side-load" onto the device via the web interface. Either of those will be okay because it's still as good as plug and play; there's no reconfiguration or recompilation to do in the final/end-user versions!

Of course developers want the source code anyway, as this is an SDK not just a set of sample applications. But we will provide an easy way to install the applications stand-alone too. For example they could be very useful for hardware tests and diagnostics because they are much easier to use than the orignal Linux tools.

Links to download the development tools and basic instructions what to install are provided by Microsoft here:

[Visual Studio 2015 Community Edition with IoT Tools Download Page for Raspberry Pi 2](http://ms-iot.github.io/content/en-US/win10/SetupPCRPI.htm)
