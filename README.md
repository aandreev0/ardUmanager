Authorship: Andrey Andreev, Scott E. Fraser
aandreev@usc.edu

Translational Imagign Center @ Univeristy of Southern California

# ardUmanager
custom code for arduino-uManager integration

Scope: 2P laser intensity control via Pockels cell voltage signal

#Documentation (taken from SEF basecamp project)

Test conditions:
* uManager versions: 1.14, 2.0, ...
* Arduino Uno (US version)
* Hamamatsu camera

##Intro
Arduino+uManager provides robust and cheap way to control analog outputs of the Arduino card.
We use this capability to control Pockels cell that modulates 2P laser beam intensity at the sample (Pockels cell rotates polarization, we put polarizer at Pockels cell output)

Latest release-quality code can be pulled from public GitHub repo branch called "release" at https://github.com/aandreev0/ardUmanager/tree/release
Development branch is "master" which contains either untested or otherwise under-supported version of code.

##How to request help
Figures and most recent docs can be found on GitHub in `documentation/` folder
You can create Issue request on Github if something doesn't work, or checkout code, make change and file merge request

##How to prepare Arduino
We use Arduino Uno with custom-build shield, which hosts Digital-to-Analog Converter (DAC) chip.
Parts:
* Arduino Uno
* MCP4725 Breakout Board - 12-Bit DAC w/I2C Interface (soldering required) https://www.adafruit.com/product/935
* Shield (soldering required) https://www.adafruit.com/product/196
* BNC leads to connect devices together via coaxial cabling http://www.mouser.com/ProductDetail/Pomona-Electronics/4969/?qs=sGAEpiMZZMuYRc8VvBrIhhTUnUOvodOEa01vcrji9e0%3d

See __Figure 1__ for schematics/soldering.

Code for Arduino is based on code from uManager page

https://micro-manager.org/wiki/Arduino#Arduino_Software

https://valelab.ucsf.edu/svn/micromanager2/trunk/DeviceAdapters/Arduino/AOTFcontroller/AOTFcontroller.ino

To burn firmware to Arduino, one should install Arduino software/IDE, install it (https://www.arduino.cc/en/Main/Software), open file from repo called umanager-firmwave.ino

This file has to sit in synonymous folder (umanager-firmwave/umanager-firmwave.ino)

Connect board, select proper board from menus Tools->Board and Tool->Port, and upload software to the board

## How to install Arduino board to work with uManager
__Figure 2__

1. Connect card via build-in or through USB hub
2. Add card using Hardware Configuration Wizard
3. Find Arduino Hub, add device
4. Click on "Scan" Let uManager find proper COM port (/dev/ttyxxx on *nix) to connect to Arduino
5. Add all peripherals for Arduino-Hub
6. Proceed with other devices, save config

## How to prepare and connect Hamamatsu camera
Pick Trigger output from camera
In uManager device properties you should have following setting: (__Figure 4__)

```* HamamatsuHam_DCAM-OUTPUT TRIGGER KIND [0] ==> EXPOSURE```

```* HamamatsuHam_DCAM-OUTPUT TRIGGER POLARITY [0] ==> POSITIVE```

Number [0] is id of the trigger channel. Our camera has 3 trigger outputs, numbered 0...2
You only will use one of those for this Arduino.

## How to connect Camera, Pockels cell, and Arduino using BNC cables
See __Figure 5__ for schematics

1. Camera Trigger connects to Input marked "D2"
2. Pockels cell Input connects to Arduino's output marked "DAC"

## How to setup proper Groups/Presets to control Arduino from uManager
See __Figures 3__

1. Click on "+" button near "group" in Configuration setting sub-window
2. Check box that contains "Arduino-DAC1-voltage", add name to group (like "Pockels level")
3. Set desirable level of 2P intensity from 0..5V range
Be careful that 100% of 2P power will be produced by just 1V setting.

One can also (additional to continuous settings) add several discreet levels for Pockels cell voltage using uManager "Presents"