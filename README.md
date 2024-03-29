# HDR-3-DIRS  
  

This is a Windows app that interfaces with the Hawkeye camera and
produces a sequence of bracketed images that can be postprocessed in
HDR utility.
It produces images with 3 exposures. The first image has exp = normal Second image has exp = normal + (increment)
and the third image has exp = normal - incremenet. The images are stored in 
separate directories. The directories can be combined is several ways to generate the 2 exp or 3 exp videos.  
alternatively, the directory with not exposure change can be used to create a regular non HDR video.  

This High Definition HDR app was made specifically to work with the Imagingsource UX226 cameras.
A new version will be availabale to work with the BUC02 camera as well.  
Please contact the author for details.  
The Hawkeye MSP430 firmware was also modified to provide the 3 exposure selection.
The selection is done by the REWIND switch in slow speed mode. The REWIND switch is normally used to rewind film with the RUN switch turned off. With the RUN switch ON, the REWIND switch has no affect normally, but in this version it
is used to switch to HDR mode.
REWIND OFF - 1EXP mode
REWIND ON - 3EXP mode
Important note: When stopping the Hawkeye turn the RUN switch off before the REWIND switch. Failing
to do that may result in the last dark image missing.

Note: Hawkeye board V12 or higher and MSP FW are required for proper HDR operation. The firmware is available here. Download the workspace_v9.zip file and extract it somewhere in the local drive.
Then in Code Composer use File->Switch Workspace to load in this new wokspace. The project that you want to build is
hawkeye_12_hdr_3dirs

Run instructions:
To run the sw download the files from this repo and go to the HDR/bin/Release
directory and run the HDR1.exe file by double clicking on it. The Device Settings window will pop up.
Select the device and resolution as required and click OK.
The Device Window will close and the app window will pop up.
![image](https://github.com/vintagefilmography/HDR-3-DIRS-EX226/assets/48537944/a878edd1-8e4e-4464-9561-2d7abccedd84)

The directory path of the output images is set by the Path button.
If using enfuseGUI the numbering should start at 1000 because
enfuseGUI does not sort the images properly. This can also come in handy if the scan is stopped and restarted.
This needs tot bee verified.

Then click on Trigger buton a few times.
It will go from red to white. Leave it white for free run.
Click Start.
The camera preview will be displayed in the preview window.
Click on ZoomIn and ZoomOut buttons and set the zoom as needed.
Click on Settings button.
The familiar camera settings will pop up.
Set the color, partial scan etc  (note partial scan works only if the initial re selection was set to lower than fill rez)  
Turn auto exposure off the exposure tab. The HDR runs in manual exposure. Set Exp Inc to the number of stops times 3. I.e. for 1 stop decrement use number 3. Do a test run to see if the decremnet is large enough to reduce the overexposure of bright areas.
You can leave the settings window open or you can close it if no more adjustments are anticipated. Click again on the Start button. It should go white.
Click on SaveConf button to save the device settings.
Make sure the Start buttoon is not active, otherwise an exception will pop up.
The next time when you run the app you can use the LoadConf to retrieve the settings.
The settings are stored in device_state.txt file in the same directory where the app resides.
Select the bit depth by clicking the Bit64 button. Do not do this if you want to leave the default 32 bit. The Bit64 button will turn red indicating 64 bit mode.
Make sure that the Start button is not active when you are doing this otherwise the app will crash.
Essentially the start botton starts the live display and the bit depth and some other camera critical settings can not be changed in Live mode. Hit Load Exp to load the exposure value from the camera to the app. Hit Disp Exp to see the actual exposure. This can be done at any point during scan. Most of other settings can be changed in Live mode however.
The SaveTiff button also can be set in Live mode.
If SaveTiff button is active it will switch from default Jpeg format to Tiff.

Click on Trigger button to activate external trigger.
Click on ImgSave.
It should turn red.(Keep an eye on this button when testing.
It could flood your drive with images if trigger is left on FreeRun.
Click on Start Button.
The app is now ready for images.
Run the scanner. Scan Restarts This app has the restart capability i.e. the scan can be stopped, the settings can be changed and then the scan can be continued from the point where it stopped. The Hawkeye firmware stays synched with the app so that the HDR sequence stays in sync. To restart the scan just turn off the Hawkeye RUN button. Turn off the app Start Button, Trigger and ImgSave in that
order. Turn Start back on and adjust the exposure etc to your liking.
Once done turn Start off and then Trigger and ImgSave back on and finally Start back on. Turn the Hawkeye RUN switch back on and the scan will resume back from where it stopped the HDR image sequence will continue ininterrupted.
It should be noted that the path and the image numbering sequence can be also changed before the Restart if that is required but should not be done while the can is running.

BTW - you can run the scanner without running the Hawkeye. With the Hawkeye led on you can hit the
sw trigger and that will save the images.
Snap9 The trigger has to be on and then the sw trigger button can be used to trigger the camera.
This can be handy for troubleshooting of the issues.

Note, once the scan process is complete you can use an app to convert the high and low images into HDR.
The app that seems to work very well can be obtained from the following site: http://enblend.sourceforge.net/
Download enfuse and use the enfuse-hdr.bat (included here in the repository) script to run combine the images.
Alternatively, use the enfuse GUI. It is easier to use and it accepts many images. http://software.bergmark.com/enfuseGUI/Main.html There is also another app that might be worth looking into. https://skylum.com/aurorahdr

Additional notes:
If tiff file transfer, is unreliable (occasional dropped frames), switch to jpeg. The difference is not perceptible not even on a large screen.The jpg files can be converted to a combined HDR tiff file, if needed, with Enfuse.

Can use basic script batch file to process 2 exposure HDR files in separate folders using Enfuse v4.2. Note: Work in progress to add 3 exposure mode to the batch file. http://enblend.sourceforge.net/index.htm

For single dir HDR use EnfuseGUI v2.1.3, this gui uses Enfuse v4.0 and makes the process very simple. http://software.bergmark.com/enfuseGUI/Main.html

If one wanted to use the later command line Enfuse v4.2, there are included Droplets v0.2.1 by Erik Krause, there is also a newer v0.4.2 available, with added extra options like EXIF copy feature (not needed for this simple application). Because some changes have been made to Enfuse over time, these Enfuse droplets no longer function as they are, but with a very minor one line change, they still work.
Change line from:- set enfuse_additional_parameters= --wExposure=1 --wSaturation=1 --wContrast=0
To :- set enfuse_additional_parameters= --exposure-weight=1 --saturation-weight=0.2 --contrast-weight=0 --hard-mask
http://www.erik-krause.de/enfuse_droplet.zip
https://groups.google.com/g/hugin-ptx/c/3VuXOjVqZPk

The TestBW button can be used to gather the image transfer process stats. If the stats show dropped transfers then
your system is probably slow or you have a USB speed issue or poor quality USB cable or a few cables patches together. Try running the performance tests on your computer. If it all fails then try to scan with Jpeg instead of Tiff.
Possibly switching from 64 to 32 bit may help.
