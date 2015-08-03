#Windows IoT Development

This page describes how to start development by installing Windows IoT on your device, Visual Studio on your PC, then finally how to clone, build and run the code.

##Prerequisites

Use the links on the [Download Page](http://docs.emlid.com/Downloads/Windows-IoT) to download the current Windows IoT Core image and Visual Studio 2015 Community Edition with Tools.

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
