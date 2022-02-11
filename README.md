# CL-260A-files
Files and notes for running an Ultimaker 2 clone 3D printer

Thanks to SirGeekALot for most of the Marlin tweaks.
Notably, my extruder PID settings are very different from his, though.
So you should find your own, with M303 C8 E0 S205 U1
| Original					        | SirGeekALot					      | Mine |
| :---: | :---: | :---: | 
| #define  DEFAULT_Kp 22.2	| #define  DEFAULT_Kp 29.79	| #define DEFAULT_Kp  14.06 |
| #define  DEFAULT_Ki 1.08	| #define  DEFAULT_Ki 2.31	| #define DEFAULT_Ki   0.64 |
| #define  DEFAULT_Kd 114	  | #define  DEFAULT_Kd 95.90	| #define DEFAULT_Kd  76.44 |


### To send the tweaked Marlin firmware to your CL-260A:

0. Ensure you have the same hardware, including:
Arduino Mega 2560  
RAMPS 1.4 board  
Heated bed  
A4988 stepper drivers  
128 x 64 LCD screen <-- If you do not have this, comment out line 2378 in Configuration.h:  
    //#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER

1. Install drivers for the CH340 USB-serial chip
	Copies of the driver files are in "CH340 drivers"
	The OEM's drivers page: http://www.wch.cn/download/CH341SER_EXE.html

2. Install Arduino IDE
	https://www.arduino.cc/en/software

3. Test the connection to the Arduino by uploading the blink example program

4. Download Marlin 2.0.x: https://github.com/MarlinFirmware/Marlin
   I used Marlin 2.0.9.3

5. Replace Configuration.h and Configuration_adv.h

6. Run Marlin.ino

7. Upload. This takes several minutes.

8. Run M303 C8 E0 S205 U1 to find your own extruder PID settings.  
	This already saves the settings in EEPROM, but better to keep a copy.  
	Watch the terminal near completion so the temp sensor readings
	don't push the PID tuning output past the end of the log
	before you can read them

9. Add your PID settings to your own copy of Configuration.h

### To use import the Cura settings:

1. Install Cura
	https://ultimaker.com/software/ultimaker-cura
	I used Cura 4.13.1
  
2. Help -> Show Configuration Folder

3. Copy all the files from "cura" into your configuration folder

4. Note that my starting G-code includes a "brush wipe" step
  where you may strap a toothbrush in that corner to wipe the
  nozzle over it before each print.

### CL-260 manual (original information pack) is what the manufacturer provided.
It uses Marlin 1.x (incompatible with 2.x), has a similarly incompatible Cura file.  
They suggest Repetier but with a Raspberry Pi and Octoprint you can untether
the printer from your computer.

### Other changes:
Better 24V power supply  
    Cover for PSU: https://www.thingiverse.com/thing%3a119109  
        https://web.archive.org/web/20161110102835/http://www.thingiverse.com/thing:119109   
        Here is a similar one: https://www.thingiverse.com/thing:40377  
    Bracket for PSU: CL-26_PSU_Bracket.stl  
Add grounding wires to the frame  
Add spool holder that fits onto provided bolt: m5-spool-holder.stl  
Case for 12864 LCD screen: https://www.thingiverse.com/search?q=12864+case&type=things&sort=relevant  
Longer ribbon cables to mount LCD screen on top T-slot frame  
Messy wiring  
Piece of tempered glass that is almost the right size  

### Potential changes:
* Use RAMPS 1.6 board  
* Use better stepper drivers. quieter operation, maybe smoother  
* DRV8825 - do not use. requires TL smoother for use at low speed  
* TMC2130  
* TMC2208  
* TMC2209 - less hot than TMC2208.  
  Support for sensorless homing! Very cool. Maybe it puts too much force on z-axis, though;  
  you don't want to delevel the bed or damage the bed or nozzle.  
  Still, it could potentially allow mesh bed leveling.  
  The TMC2209 uses UART mode for sensorless homing, to use it will require some wiring.  
  You can use one-to-many wiring, where all the stepper motors share a bus and are configured with unique 2-bit addresses  
  The extruder should be run with spreadCycle, axis motors run with stealthChop  
  These two modes are configured by bringing a couple pins to VCC, ground, or leaving them open on the driver  
