# Electronics

These are a short form list of steps you will need to go through to get your machine operational. There are more information and links for many of the steps below the list.

Electronics assembly steps:

* Mount the 36V power supply into the back of the gantry where the ventilation holes are
* Mount the VFD to the gantry
* Install the emergency stop 
* SAFETY WARNING! Wire the incoming 220V to the VFD and the Power supply 
* Test the emergency stop
* Attach the arduino to the controller card
* Test if the card works by connecting to it via USB
* Program the arduino with GRBL
* Connect with UGS and do the firmware settings, use the correct size of your machine!
* Make the cables for the motors and switches
* Thread them through the holes in the gantry to where they should go. Mark the ends by the controller with X, Y and Z.
* Make a power cable from the power supply to the controller, don't power yet.
* Disconnect all cables from the controller
* Power the controller, check for smoke. A LED should glow on the back if all is well.
* Unscrew the Z motor assembly from the machine, we will use it to test the electronics.
* Connect the power connector (only!) from the Z motor cable to the loose motor.
* Connect the Z motor cable to the controller. Check if the motor turns on and gets stiff. If not, check what is wrong.
* Connect also the signal plug on the Z motor. Connect the USB to the arduino. Open UGS, configure the firmware via UGS to 80 steps per mm. Try jogging Z. The motor should spin!
* Connect the other cables to the controller and connect the power connectors (Only!) to the motors. Check that all turn on.
* Try jogging the other axis small distances.
* Open UGS, connect, go around the machine and test each limit switch, you should se an alarm in UGS for each.
* If all works you can reconnect and retune the Z-axis. If it does not, use the loose motor to figure out what is wrong.
* Mount the probe plate
* Run a wire to the probe plate and another to the spindle for the probe connector
* Program the VFD according to its manual and your spindle specifications.
* Wire the control wires for the VFD (refer to your VFD manual and the HRBL schematic)
* Test the VFD
* Mount the controller in the gantry.
* Attach the back of the gantry.
* Clean of the table
* Test jogging to all extremes, no snags or strange sounds. There should be no rattling (loose tuning of wheels or pinions) or squeeking, that means a pinion gets too tight for some reason. Retune if necessary. 
* Set up a homeing sequence, before running it, jog the machine to the probe plate and test that it sends a probe alarm when you short it with a tool. [Use this gcode file as a template](Settings_backup/home_and_probe_grbl.gcode). Run one line at a time and find out the values you need for your machine and save it in a file of your own. 
* TIP: Add the homeing routine to your UGS macros list by removing comments and separating the commands with semicolons (;). Keep the file for it's human readable comments.
* Double check the machine size settings in the firmware from your homeing switches and consider using soft limits.
* See the [wiki instructions for how to set up a job](https://github.com/fellesverkstedet/fabricatable-machines/wiki/How-to-use#humphrey)
* Run a test job, have a fire extinguisher at hand, wear safety googles and ear protection.
* You now have a working machine!
* Please help the project by adding to and improving it!
* Post [issues](https://github.com/fellesverkstedet/fabricatable-machines/issues) to ask questions or suggest improvements

[Back to the overview](Readme.md)

## HRBL-Controller

The HRBL-controller is a optoisolated connection shield for an Arduino Nano running the GRBL CNC control firmware. 

For factory made PCB see [HRBL arduino NANO shield controller card](https://github.com/fellesverkstedet/fabricatable-machines/tree/master/hrbl-shield/README.md)

### LEGACY Humphrey v4 HRBL controller card - Milled PCB ONLY! 
![Connections](https://github.com/fellesverkstedet/fabricatable-machines/raw/master/hrbl-shield/img/connections.JPG)

Spindle controls schematic Milled PCB ONLY!: 

![spindle controls](https://github.com/fellesverkstedet/fabricatable-machines/raw/master/hrbl-shield/img/spindle_controls.JPG)

* The FOR pin will be shorted to the DCM(COM) pin (in that direction) when the controller wants to turn the spindle on
* The VI is an unfiltered 5V PWM signal with a 1kHz frequenzy, the 5V source is the controller (voltage reference).
* ACM(GND) is a signal ground reference (for the PWM).

### LEGACY Humphrey v3 HRBL controller card
![Connections](img/electronics/hrbl_connections_all.JPG)

### Programming the arduino with the GRBL firmware

* [Download and install arduino if you don't have it](https://www.arduino.cc/download_handler.php)
* Download the [GRBL files for arduino IDE programming with spindle enable configured](https://github.com/fellesverkstedet/fabricatable-machines/raw/master/humphrey-large-format-cnc/GRBL_Spindle_ENABLE.zip)
* Manually unzip the file in a folder (don't skip this step)
* Connect your arduino to your computer
* Make sure your board is set to the appropriate Arduino board in the Tool->Board menu and the serial port is selected correctly in Tool->Serial Port. (If it does not show up, try installing these [arduino clone USB drivers](https://github.com/KubenKoder/Arduino/blob/master/USB%20driver/CH341SER.EXE) )
* Now we need to add the GRBL files as a library to the Arduino program
 * In the menu go to **Sketch > Include Library** and select Add .ZIP Library. (The Add .ZIP Library command supports both a .ZIP file or a folder.) Add the folder that you unzipped.
 * You can confirm that the library has been added. Click the Sketch drop-down menu again, navigate to Include Library, then scroll to the bottom of the list where you should see grbl.
* In the menu click: File > Open, navigate to the folder Examples->Grbl, and select GrblUpload.
 **Do not alter this example in any way! Grbl does not use any Arduino code. Altering this example may cause the Arduino IDE to reference Arduino code and compiling will fail.**
* Press the round arrow button to compile and **upload** Grbl to your Arduino.
* *If you can't upload try changing this in the menu: Tools > Processor > Old Bootloader*

More information about GRBL [How to use GRBL](https://github.com/gnea/grbl/wiki)

## Motors

Humphrey uses 4 Nema24 closed loop stepper motors with integrated drivers.

![motor](https://github.com/fellesverkstedet/fabricatable-machines/raw/master/hrbl-shield/img/ihss57-integrated-closed-loop-stepper-from-jmc.jpg)

[Motor manual](https://github.com/fellesverkstedet/fabricatable-machines/blob/master/hrbl-shield/dev_files/integrated_stepper/20160528161106_17875.pdf)

### Connections

Info:

The enable pins can be ignored, then they are always read as enabled. We either want the motors to hold position or be unpowered by us cutting all power. 

### Programming

Set the dip switches on the steppers to 001100 to use 3200 steps per revolution. 

*IMPORTANT* Flip the last switch on one of the Y motors to make them spin in opposite direction.

Info:

The pinion rolls 40 mm per revolution so that and 3200 steps per revolution gives us 80 microsteps per mm (5 fullsteps per mm). We will program this into the firmware of the GRBL controller.

## Motor and limit switch cable connections

Make 4 of these, the ribbon cables should be at least 1.6 meters long each. The plugs go into the controller card and into the motors.

![Connections](img/electronics/Connector_guide.jpg)

## Spindle and Variable Frequency Drive

The spidle will spin 24000 rpm when given 400hz from the vfd.

It should not be run slower than 10000 rpm since it's air cooled, so set a minimum of 165 hz

*Always check what the VFD is programmed to output before tuning it on with the spindle attached!*

### China-VFD

[Settings advice](https://www.cnczone.com/forums/spindles-vfd/346620-huanyang-vfd-spindle-accelerate-amp-decelerate-settings.html)

[Recommendations](http://www.woodworkforums.com/f170/tips-newbie-huanyang-vfd-users-96380)

[Guide for Mach3](http://www.kronosrobotics.com/hy02d223b-vfd-type-1/) 

Set these parameters on the VFD

* pd000 = 0 
* pd001 = 1 
* pd002 = 1
* PD005 = 400
* pd007 = clear
* PD011 = 165
* pd014 = 2
* pd015 = 2
* pd070 = 1
* jumper leftmost (VI to center)

## Commanding computer

* Download nightly build of [UGS Platform](https://winder.github.io/ugs_website/) and use a computer (PC, MAC or Linux) to control the machine
* (The stable build )

### Fixing the "missing java" problem with UGS
If your computer suddenly claims that it can't find your Java installation you can help it by telling it where it is.
* First install the latest [Java](https://www.java.com) version to be absolutely sure you actually have it.
* Go to where the folder which contains the UGS program files, it's called ugsplatform. *It will be where you unpacked it, if you don't know where you put it, then it's probably in your downloads folder.* 
* Open the ugsplatform/etc subfolder
* Edit the ugsplatform.conf in your favorite text editor (notepad etc..)
* Find the line starting with #jdkhome 
* Replace it with this line: (remove the #)
```
 jdkhome="C:/Program Files (x86)/Java/jre1.8.0_221/"
```
* Double check that you have java in that folder, if it is somewhere else, adjust the path to fit.
* UGS should now work
* Do yourself a favour and create a desktop shortcut to the UGS start file **ugsplatform/bin/ugsplatform64.exe**

OR

* Use a raspberry pie and octoprint to set up a web based controller on your local network [instructions](https://github.com/fellesverkstedet/fabricatable-machines/wiki/Electronics#octoprint-with-grbl-plug-in-in-progress)

* Use this [file](https://github.com/fellesverkstedet/fabricatable-machines/blob/master/humphrey-large-format-cnc/humphrey_v2/Settings_backup/home%20and%20probe.gcode) to set up a good homeing-routine.
* RECOMENDED! Use a post processor that always trigger the homeing routine to make it impossible to forget.

## User guide, how to mill with Humphrey

The user guide for how to mill with Humphrey is on the wiki part of this page.

[Wiki link](https://github.com/fellesverkstedet/fabricatable-machines/wiki/How-to-use#humphrey)

Please help the project by adding to and improving it!

CONGRATULATIONS! That is all you need to get your Humphrey going! We hope you want to add to these instructions and post your questions and ideas for improvements in the [git issues for this repo](https://github.com/fellesverkstedet/fabricatable-machines/issues).

And if you design your own improvements to the design or nice add-ons, please let us and everyone else know in the [git issues for this repo](https://github.com/fellesverkstedet/fabricatable-machines/issues)! 

[Back to the overview](Readme.md)

### OPTIONAL: HISU motor calibration

A HISU can be used to increase motor torque, remove smoothing and set the max error value for the closed loop.

![Hisu](https://github.com/fellesverkstedet/fabricatable-machines/raw/master/hrbl-shield/dev_files/integrated_stepper/6C3A13B4-B353-4DFC-950C-6E98B35EEC22.jpeg)

[HISU on aliexpress](https://www.aliexpress.com/item/HISU-for-Andriy-Kyrychenko/32805819281.html?spm=2114.search0104.3.1.6c2661d1JJVdOY&ws_ab_test=searchweb0_0,searchweb201602_3_10065_10068_10059_5015212_10696_100031_10084_10083_10103_5015812_451_452_10618_5015112_10307_5015912,searchweb201603_56,ppcSwitch_5&algo_expid=bcced743-392b-477b-ba84-0d559d9ffe03-0&algo_pvid=bcced743-392b-477b-ba84-0d559d9ffe03&priceBeautifyAB=0) to adjust motor settings

![how to HISU](https://github.com/fellesverkstedet/fabricatable-machines/raw/master/hrbl-shield/dev_files/integrated_stepper/HTB1.IttbURIWKJjSZFgq6zoxXXak.jpg)
 
 _How to HISU_
 
 [Default values for the motor](https://github.com/fellesverkstedet/fabricatable-machines/blob/master/hrbl-shield/dev_files/integrated_stepper/Default%20values.txt)

See the [motor manual](https://github.com/fellesverkstedet/fabricatable-machines/blob/master/hrbl-shield/dev_files/integrated_stepper/20160528161106_17875.pdf) for which values to change.

P16 = accepted error in degrees (ca) before throwing an error and stopping, requiring a power-off-on to reset. Try a low setting like 5 to detect problems early! 

P19 = set to 0 for no extra speed smoothing in the motor.

P8 = sets holding torque (max 40)

P9 = sets driving torque (max 30)

P8 + P9 = gives total torque. They can be set to max for max performance but setting them to ~70% saves on pinion wear when accidentaly crashing the machine into endstops etc. 

It’s not worth messing with the kp, ki and damping values unless you know what you are doing. We think they control how the stepper driver drives the current levels between steps, not how it uses the closed loop position. Changing them seems to changes the ”squeek” of the motors but not much else. Remember that is a closed loop stepper and not a servo. 

When you are ready to test the machine see [How to run a milling job](https://github.com/fellesverkstedet/fabricatable-machines/wiki/How-to-use#humphrey)]

[Back to assembly main page](Humphrey_how_to_assemble.md)

[Back to Humphrey overview](README.md)

