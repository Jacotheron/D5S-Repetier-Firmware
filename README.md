# Repetier-Firmware for D5S - the fast and user friendly firmware

This repository contains a forked version of the original Repetier Version, which is specifically tailored for the Wanhao D5S series printers. For this to work the pins for the Ultimaker board was modified

## Warning

This firmware is Beta software and can have bugs. It is not recommended to run this firmware without knowledge of the firmware and how it interacts with your machine.

## Changes
* Made to work with a D5S Mainboard and D5S screen (the screen uses non-standard pinouts and other requirements).
* Made to use a 100k NTC 3950 thermistor in stead of the stock k-type thermocouple. In order to use this, you have to create a voltage divider board/circuit (I made mine from standard stip board - tutorial coming soon).
* Features that I do not use, was disabled. You are welcome to configure these in the firmware.
* The Configuration file is not Assistant Tool ready, but it contains full comments.
* Buzzer have been disabled, is was getting irritating (but you can enable it).
* Stopping a print will Home the head in the X direction (I hated the head just remaining where it was), before disabling the steppers.
* EEPROM is enabled and thus a lot of values can be changed using Repetier Host. For subsequent changes, you may need to set the EEPROM mode to 1 or 2 (the number it is not currently).

## Documentation

For documentation please visit [http://www.repetier.com/documentation/repetier-firmware/](http://www.repetier.com/documentation/repetier-firmware/)

In order to compile this, you will need Arduino IDE (the latest is able to compile this firmware). The D5S mainboard is an Arduino Mega 2560 compatible board (you will need to select it from the Tools menu).
The firmware Arduino File is located in "src/ArduinoAVR/Repetier/repetier.ino"

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

## Changelog

See changelog.txt
