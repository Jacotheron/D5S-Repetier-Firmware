# Repetier-Firmware for D5S - the fast and user friendly firmware

This repository contains a forked version of the original Repetier Version (0.92.9), which is specifically tailored for the Wanhao D5S series printers. For this to work the *pins.h* for the Ultimaker board was modified.

## Warning

**This firmware is Beta software and can have bugs. It is not recommended to run this firmware without knowledge of the firmware and how it interacts with your machine.**

## Changes
* Made to work with a D5S Mainboard and D5S screen (the screen uses non-standard pinouts and other requirements).
* Made to use a 100k NTC 3950 thermistor in stead of the stock k-type thermocouple. In order to use this, you have to create a voltage divider board/circuit (I made mine from standard stip board - tutorial coming soon).
* Features that I do not use, was disabled. You are welcome to configure these in the firmware.
* The Configuration file is not Assistant Tool ready, but it contains full comments.
* Buzzer have been disabled, is was getting irritating (but you can enable it).
* Stopping a print will Home the head in the X direction (I hated the head just remaining where it was), before disabling the steppers.
* EEPROM is enabled and thus a lot of values can be changed using Repetier Host. For subsequent changes, you may need to set the EEPROM mode to 1 or 2 (the number it is not currently).

## Configuration

You are free to make changes to the *configuration.h* file, for example setting up special features. A few useful changes:

### Decoupling
Decoupling (shown as DEC in the place of the temperature after triggered) is a setting with the purpose of detecting a temperature sensor which no longer registers a correct temperature.
This differs fom the DEF (defective) in that the sensor is still giving possible temperature readings. By default DEC will mark the sensor as decoupled, enable Dry Run (which ignores the extrude commands) and shut down the heater (to protect you from a possible overheat/fire).

Decouple testing have the following settings in the firmware:
* **DECOUPLING_TEST_MAX_HOLD_VARIANCE** - If the temperature sway more than this amount from the target temp (after reaching it), the sensor will be marked as Decoupled. Current 20
* **DECOUPLING_TEST_MIN_TEMP_RISE** - When you set a target temperature, more than **DECOUPLING_TEST_MAX_HOLD_VARIANCE** above the current temperature, the sensor needs to increase at least this amount of degrees in a set time (**EXT0_DECOUPLE_TEST_PERIOD**), or it will be marked as decoupled. Current 1
* **KILL_IF_SENSOR_DEFECT** - What should be done when a sensor becomes defective/decoupled during a print. Values are 0 and 1; Current 1 to kill print.
* **EXT0_DECOUPLE_TEST_PERIOD** - Time allowed for the **DECOUPLING_TEST_MIN_TEMP_RISE** or it will be marked as decoupled. Current 20000 (value in milliseconds). Remember that the rasie needs to be from what it was when the new target was set (if it is cooling, it needs to make up for the cooling since as well).

If you are printing a few print jobs after each other, it is best to set the nozzle to preheat between the prints while you remove the previous part and prepare the surface for the new print. Based on my testing, if the nozzle is cooling between 150 and 80C it is cooling way too fast to start a new print and you will get the Decoupled sensor error. If it cooled this far, I just let it cool to outside of this range and then set it to try again.

While it is possible to disable this decouple test (like in the original firmware), it is a great risk to disable it and thus not recommended.

### Temperature
A stock Wanhao D5S printer comes with a thermocouple, that is amplified by a tiny board in the black pipe right above the printer head. While this is the best place on this specific printer for this board, it is still not in an ideal place and may break (ideally it should not bend close to this board). Moving the board further away from the head, is not an option since a thermocouple wire may not bend regularly.

This firmware is set by default to rather use a thermistor (cheaper than the thermocouple) for temperature sensing. This requires a small change to your mainboard, or create a separate small board that does the exact same thing as the other change (see below for more details).
A thermistor is Sensor type 1, while the stock thermocouple amplifier is a type 100 - there are a lot of other options too, see the comment in the configuration.h file.

* **EXT0_TEMPSENSOR_TYPE** - the setting which controls which sensor type to use.

Other useful temperature related settings.

* **MIN_EXTRUDER_TEMP** - The minimum temperature you want. If you need to print at lower temps, you can lower this.
* **UI_SET_PRESET_EXTRUDER_TEMP_PLA** - The temperature you want PLA to preheat to.
* **UI_SET_PRESET_EXTRUDER_TEMP_ABS** - The temperature you want ABS/PETG or other high temp materials to preheat to (remember the nozzle's maximum safe temperature is 250).
* **UI_SET_MIN_EXTRUDER_TEMP** - Minimum temperature that can be set by the UI.
* **UI_SET_MAX_EXTRUDER_TEMP** - Maximum temperature that can be set by the UI.

### Heatbed
The older mainboards, which have 3 temperature sensing and 3 heater controlers are able to control a heatbed from the firmware (similar to what the guys with a D4, Di3, and D6 can do). Obviously you will nead to buy a heatbed and a few other components to do this. Most importantly you will need a Power Expander (available from reprap.me) since you will need a separate power supply dedicated to the heatbed (a 24V should be good, and for this size at least 200W) - the job of the Power Expander is simple: switch the power to the heatbed on and off, from the commands received from the mainboard.

If your board suports it, and you have everything connected, you can set th following settings:
* **HAVE_HEATED_BED** - set to 1 to enable
* **HEATED_BED_MAX_TEMP** - heatbed max rated temperature (usually around 100C)
* **HEATED_BED_SENSOR_TYPE** - what temp sensor is on the heatbed.

### Printer Max Size
This firmware is configured to accomodate both the D5S Mini and a full D5S. This simply means that the printer will by default allow commands up to a Z of 590mm high. Having it firmware limited to this is not an issue for people with the Mini, since your Slicer needs to be configured to not give a command more than what your printer can actually handle (The Mini only supports a physical maximum of 200mm high). If you would like to firmware restrict your printer, you can set change it:
**Z_MAX_LENGTH**

### EEPROM
The EEPROM is enabled in this firmware. This allows you to easily change some key settings, perform a PID tune and save the values and other very nice functionality (like total printing time and filament used). However some settings will not overwrite the EEPROM if you flash the firmware again (after you made changes). To start with a clean EEPROM, change the **EEPROM_MODE** value.

### Languages
This firmware by default compiles with support for all Repetier supported languages. This is very nice, since you can easily select your language from the configuration in the printer menu and everything is translated, but it uses more storage on your printer. You can disable all the languages you do not need and they will disappear from the printer (you can re-enable them and just flash the new firmware to your printer).

### Buzzer
The buzzer can be enabled by setting **FEATURE_BEEPER** to 1.

## Documentation

For documentation please visit [http://www.repetier.com/documentation/repetier-firmware/](http://www.repetier.com/documentation/repetier-firmware/)

In order to compile this, you will need Arduino IDE (the latest is able to compile this firmware). The D5S mainboard is an Arduino Mega 2560 compatible board (you will need to select it from the Tools menu).
The firmware Arduino File is located in "Repetier/repetier.ino"

For now the firmware will not be in a ready to flash file format - you have the source to help debug and contibute to the development.

While it is possible to make the D5S control a heatbed (through a Reprap.me Power Expander and separate power supply), this can only happen on the older mainboards (where there are more than one temperature sensing and control outputs). My mainboard can do this, but to keep this usable by others I will create a separate branch for the added heatbed.

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

## Introduction

Repetier-Firmware is a firmware for RepRap like 3d-printer powered with
an arduino compatible controller.
This firmware is a nearly complete rewrite of the sprinter firmware by kliment
which based on Tonokip RepRap firmware rewrite based off of Hydra-mmm firmware.
Some ideas were also taken from Teacup, Grbl and Marlin.

## Features

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

For both methods you will a need a thermistor. We recommend getting one on the M3 stud, since they can easly be fastened without fear of damaging it (the glass bead thermistors are extremely fragile).

### Option 1: Solder a 4.7k Ohm resistor to the mainboard
This is the very simple option, if you are happy to solder onto your mainboard. You will need:
* a surface mount 4.7k Ohm resistor.
* Solder tools and wires

1. Locate the R15 (on newer mainboards) - it should be right at your TEMP1 connector (where you plug in the wire leading to your temperature sensor). It is close to the C14 capacitor. There should not be any resistor on it - we will now add it.
2. This R15 sits between the +5V and the S coming from the sensor.
3. Now solder the 4.7k Ohm resistor to this spot made for it. Be carful not to damage the other parts or keep heating a spot for too long - rather get help than damage the mainboard.
4. Remove the thermocouple and its amp from the nozzle and the pipe respectively, and connect your 100k NTC thermistor to the nozzle.
5. The thermistor needs to be placed between the Ground (aka -) and the S (signal), so you can modify your 3 wire cable to accomodate only the outer 2 wires to the thermistor (center is the +). A thermistor does not care which side is the Signal and which is Ground.

![alt tag](https://raw.githubusercontent.com/Jacotheron/D5S-Repetier-Firmware/master/img/r15-location.jpg)

### Option 2: Create a Voltage Divider board
This is also a simple option with the added benifit of not modifying your mainboard in any way.
You will need some Strip Board (or other compatible pc board), a 4.7k Ohm resistor, a jumper.

![alt tag](https://raw.githubusercontent.com/Jacotheron/D5S-Repetier-Firmware/master/img/voltage-divider.png)

Replicate this image with your stripboard:
* The dots are the holes in the stripboard.
* Blue lines are the strips of the board, and is below the board (image from the top / non-conductive side).
* The Red is the 4.7k Ohm resistor - I uses a through hole resistor for this (important it needs to be between + and S)
* The black is the jumper (connecting the signal to the side running to the thermistor) - technically if you do not want to use a connetcor for the thermistor, you can solder its wire directly to the Sig strip.
* The Beige is the connectors JST-XHP (a 2 pin for the thermistor and a 3-pin for the wire coming from the mainboard) - this is made to take the original wire directly (so that it can be undone).

A voltage divider allows the mainboard to read the resistance of the thermistor. It works by comparing the voltage over the thermistor with that of a known fixed resistor (together they will always be 5V).

# Changelog

### v1.0b5
- enhancement: embed changelog in Readme

### v1.0b4
- fixed: X and Y steps per mm
    * The D5 printers use a MXL timing belt and not the GT2 belt (as previously thought). The difference in pitch is very slightly, but can cause incorrectly sized objects (2mm for the GT2 vs 2.032mm for the MXL).

### v1.0b3
- enhancement: made the firmware multi-lingual
    * The firmware supports compiling with all the languages ready to be used. Previously only the English translation was compiled (others were disabled).

### pre v1.0b3
Made the firmware compatible and a lot of other misc changes - these are lot logged here (but can be viewed in the Git history) since they are not important. The v1.0b2 was the first to be released publicly with minor bugs.