# Repetier-Firmware for D5S - the fast and user friendly firmware

This repository contains a forked version of the original Repetier Version (0.92.9), which is specifically tailored for the Wanhao D5S series printers. For this to work the *pins.h* for the Ultimaker board was modified.

This version is old, as of 21 January 2019. For the latest version (based on Repetier 1.0.3), please visit [Repetier 1.0.3 Wanhao D5S](https://github.com/Jacotheron/Repetier-1.0.3-Wanhao-D5S)
The newer version solves compiling errors.

## Warning
While this firmware should simply work, I advise against updating to it while you are busy with an important project. Things can go wrong or simply with the new firmware the printer might be over extruding (better do those calibration cubes to get it perfect again).

After updating to this firmware, please perform the following:
* PID Tune - this allows your printer to maintain temperature.
* Extrusion Calibration - ensure you are pushing the correct amount of filament to not be over or under extruding (this firmware allows E-steps calibration and then final adjustment from your slicer).

## Changes
* Made to work with a D5S Mainboard and D5S screen (the screen uses non-standard pinouts and other requirements).
* Made to use a 100k NTC 3950 thermistor in stead of the stock k-type thermocouple. In order to use this, you have to create a voltage divider board/circuit (I made mine from standard stip board - tutorial coming soon).
* Features that I do not use, was disabled. You are welcome to configure these in the firmware.
* The Configuration file is not Assistant Tool ready, but it contains full comments.
* Buzzer have been disabled, is was getting irritating (but you can enable it).
* Stopping a print will Home the head in the X direction (I hated the head just remaining where it was), before disabling the steppers.
* EEPROM is enabled and thus a lot of values can be changed using Repetier Host. For subsequent changes, you may need to set the EEPROM mode to 1 or 2 (the number it is not currently).
* Some people report having access to variable fan speed control while using this firmware.

## Configuration

You are free to make changes to the *configuration.h* file, for example setting up special features. Options and features at:

* [Extruder Configuration](https://github.com/Jacotheron/D5S-Repetier-Firmware/wiki/Extruder-Configuration)
* [Movement Configuration](https://github.com/Jacotheron/D5S-Repetier-Firmware/wiki/Movement-Configuration)
* [Other Configuration](https://github.com/Jacotheron/D5S-Repetier-Firmware/wiki/Misc-Configuration)

While it is possible to make the D5S control a heatbed (through a Reprap.me Power Expander and separate power supply), this can only happen on the older mainboards (where there are more than one temperature sensing and control outputs). If your mainboard supports this, you can follow the guide.

* [Heatbed Configuration](https://github.com/Jacotheron/D5S-Repetier-Firmware/wiki/Heatbed-Configuration)

## Documentation

For official Repetier documentation please visit [http://www.repetier.com/documentation/repetier-firmware/](http://www.repetier.com/documentation/repetier-firmware/)

In order to compile this, you will need Arduino IDE (the latest is able to compile this firmware). The D5S mainboard is an Arduino Mega 2560 compatible board (you will need to select it from the Tools menu).
The firmware Arduino File is located in "Repetier/repetier.ino"

For now the firmware will not be in a ready to flash file format - you have the source to help debug and contibute to the development.

## Developer

The sources are managed by the Hot-World GmbH & Co. KG
It was initially based on the Sprinter firmware from Kliment, but the code has run
through many changes since them.
Other developers:
- Glenn Kreisel (long filename support)
- Martin Croome (first delta implementation)
- John Silvia (Arduino Due port)
- sdavi (first u8glib code implementation)
- plus several small contributions from other users.

This D5S specific version is developed by Jaco Theron from XiliX Technologies.

By default the printer will display "XiliX Tech" on the logo screen, you are allowed to remove or change this. This is just a small bit of credit to me - this firmware configuring, setup and testing took months to date.

## Repetier Features

- Supports cartesian, delta and core xy/yz printers.
- RAMP acceleration support.
- Path planning for higher print speeds.
- Trajectory smoothing for smoother lines.
- Nozzle pressure control for improved print quality with RAMPS.
- Fast - 40000 Hz and more stepper frequency is possible with a 16 MHz AVR.
- Standard ASCII and improved binary (Repetier protocol) communication.
- Autodetect the command protocol, so it will work with any host software.
- Important parameters are stored in EEPROM and can easily be modified without recompilation of the firmware.
- Detection of heater/thermistor decoupling.
- 2 fans plus thermistor controlled fan.
- Multi-Language support, switchable at runtime.
- Stepper control is handled in an interrupt routine, leaving time for
  filling caches for next move.
- PID control for extruder/heated bed temperature.
- Interrupt based sending buffer (Arduino library normally waits for the
  recipient to receive written data)
- Small RAM memory print, resulting in large caches.
- Supports SD-cards.
- mm and inches can be used for G0/G1
- Arc support
- Dry run : Execute yout GCode without using the extruder. This way you can
  test for non-extruder related failures without actually printing.

## Controlling firmware

Also you can control the firmware with any reprap compatible host, you will only get
the full benefits with the following products, which have special code for this
firmware:

* [Repetier-Host for Windows/Linux](http://www.repetier.com/download/)
* [Repetier-Host for Mac](http://www.repetier.com/download/)
* [Repetier-Server](http://www.repetier.com/repetier-server-download/)

## Installation

For documentation and installation please visit 
[http://www.repetier.com/documentation/repetier-firmware/](http://www.repetier.com/documentation/repetier-firmware/).

## Convert Temperature Sensor to Thermistor

I highly recommend changing from the Thermocouple to a Thermistor based temperature sensor for the following reasons:
1. The thermocouple requires a thermocouple amp. This sits right above the head in the black pipe, and is prone to getting damaged and/or breaking after a while. It can be replaced, but again the replacement is also going to break at some point in time.
2. Thermistors are cheap and accurate enough for the temperatures required for ABS and PLA filaments (going hotter than ABS temperatures on a D5 is looking for trouble and will damage the printer).
3. Thermistors require a very small change to the mainboard - a simple 4.7k ohm surface mount resitor, soldered (there is an alternative method requiring a few extra components).
4. Thermistors use a multi-core wire which can bend inside the pipe with no issues, and you do not have to check polarity (it is a simple resitor).

I have created a guide to help you to do this mod with very little issues.

[Voltage Divider Guide](https://github.com/Jacotheron/D5S-Repetier-Firmware/wiki/Voltage-Divider)

# Changelog

### v1.0rc1
- enhancement: defaults to the Thermocouple Temperature sensor - since that is what the printer comes with, it is now the default.
- enhancement: Pre-compiled a few common version of the firmware (stock with thermocouple; stock with thermistor; heatbed with thermistor) and also included the versions with Bootloaders. These are available in both .hex and .firm versions.
- enhancement: Slicer Profiles included. I recommend Simplify 3D, but the price makes it not ideal for everyone. I included a Slic3r and CraftWare profile - these may need more work (pleease feel free to send me updated versions to include).
- bug fix: Comments are back in the Configuration.H file - allowing you to easily see what can be changed and how to checnge them.
- other: EEPROM Mode is now set to 1, which will erase your current EEPROM.
- other: Performed compare with latest stable Repetier firmware version - applying changes.

With this release, I am not expecting a lot of further changes, but just to be sure there are no bugs while performing these changes I am making this an RC (Release Candidate) version.

### v1.0b5
- enhancement: embed changelog in Readme
- enhancement: offload Readme data to the Wiki, easier maintenance.
- bug fix: printer shooting to left back corner on Pause - this was something the printer just did (uncommented the line).
- bug fix: printer shooting to right back corner - this was due to too many decimal places in the Steps/MM, now reduced and tested.
- bug fix: ranom weird characters on the display - this was due to all the languages getting compiled. English is now default, select only the language you want to use.
- enhancement: Pausing the printer now raises the printer by 2mm (so the head does not melt the part). Resuming lowers with 2, to be at the correct place again.
- enhancement: Stopping a print will raise the printer by 2mm, then Home in X.
- enhancement: set E max acceleration to 3000, so that weaker stepper motors don't stall.
- enhancement: increased decoupling test time to 30 seconds, should have a bit more user friendly when starting to heat while it is cooling.
- enhancement: reduced the Max Jerk to 8, should give a bit better corners.
- other: EEPROM Mode is now set to 2, which will erase your current EEPROM (save the time/filament usage before upgrade).


### v1.0b4
- fixed: X and Y steps per mm
    * The D5 printers use a MXL timing belt and not the GT2 belt (as previously thought). The difference in pitch is very slightly, but can cause incorrectly sized objects (2mm for the GT2 vs 2.032mm for the MXL).

### v1.0b3
- enhancement: made the firmware multi-lingual
    * The firmware supports compiling with all the languages ready to be used. Previously only the English translation was compiled (others were disabled).

### pre v1.0b3
Made the firmware compatible and a lot of other misc changes - these are lot logged here (but can be viewed in the Git history) since they are not important. The v1.0b2 was the first to be released publicly with minor bugs.

# Donations

I have created this firmware, since I needed it. I continue to improve on this firmware (adding more features, optimizing other features and settings), and will also offer support on it (through the Wanhao Google Group). All of this takes a lot of time and effort.

If this firmware or my support helps you in any way, please considder a small donation.

[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=872QV2XANTMME)