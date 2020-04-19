# My Tricopter LR Build

This repository documents my build of [David Windestal's](https://rcexplorer.se/) [Tricopter LR](https://rcexplorer.se/product/tricopter-lr/).  I was collecting notes for my own reference and decided to publish them here in case others are interested.

**Warning**: Use this information at your own risk.  I am no authority on tricopters or drones in general.

Please feel free to contact me if you have specific questions about this build or a similar build you are doing.  I cannot help with general questions about building drones, or even tricopters that are significantly different from this build.  If you need help, check out the RCGroups [Tricopter forum](https://www.rcgroups.com/tricopter-drones-937/).

## Parts

* Frame: Tricopter LR
* Motors: Emax RSII-2207 - 1600kV
* ESCs: Aikon AK32 35A BLHeli32 2-4S
* Props: HQProp 8x5B and 8x5RB Bullnose
* PDB: [BabyPDB](https://rcexplorer.se/product/babypdb/)
* Servo: [BMS-210DMH](https://rcexplorer.se/product/blue-bird-bms-210dmh-servo/)
* Flight controller: Kakute F7 V1.5
* Camera: RunCam Micro Eagle Pro
* VTX: RaceDayQuads Mach 3
* Receiver: FrSky R9MM
* GPS: BN-880

Some notes on the parts:  The RCExplorer items (frame, PDB, and servo) are no longer available.  Any PDB that can power three ESCs and the servo should work.  Ideally the PDB would have a 6V output to drive the tail service a little faster.  A quadcopter AIO or 4-in-1 ESC should work, if you use a seperate BEC to power the servo.

David recommended the Kakute F4 flight controller.  My first build used one and it worked fine, but I wanted more ports for accessories so I upgraded to the F7.

## Wiring

Note: The section header refers to the component that the wire runs from, the part to the left of the arrow is the pad or pin on the component, and the part to the right of the arrow is the destination component and pad/pin.  So under "VTX", "25V -> FC B+" should be read as "from the 25V pad on the VTX to the B+ pad on the flight controller".

### VTX

* 25V -> FC B+
* GND -> FC GND
* VIDEO -> FC V<sub>0</sub>
* AUDIO -> FC T1

### Runcam Video (three pin plug)

* V+ -> PDB V<sub>BATT</sub>
* GND -> PDB GND
* VID -> FC V<sub>I</sub>

### Runcam Control (two pin plug)

* GND -> FC GND
* MENU -> FC NC

### R9MM Receiver

* SBUS_OUT -> FC R6
* SMARTPORT -> FC T4
* GND -> FC GND
* V<sub>IN</sub> -> FC 5V

### BN-880 GPS / Magnetometer

* D -> FC SDA
* G -> FC GND
* T -> FC R2
* R -> FC R2
* V -> FC 5V
* C -> FC SCL

### Fight Controller

The following are all in the plug header next to the USB port:

* M1, M2, M3 -> ESC signal wires
* B+ -> PDB V<sub>BATT</sub>
* G -> PDB GND
* I -> PDB Current
* M4, R7, T7 -> not used, wires cut

The rest are on pads:

* BUZ- -> Buzzer black wire
* 5V -> Buzzer red wire
* LED -> M5
* RSI -> Servo feeback wire (optional, most firmware doesn't support it)

### PDB

I don't have any notes on my PDB wiring.  You aren't missing much since the BabyPDB is no longer available.  If you have one and want to see how to wire it, check out David's [build video](https://www.youtube.com/watch?v=rNy6HMie_Yg).

## Firmware

I tried dRonin when I was using an F4 FC, because it took advantage of the tail servo feedback wire.  It is pretty bare-bones by modern firmware standards.  I switched to Betaflight and didn't notice any huge loss in performance (I am not what you would call a great drone pilot, keep that in mind).  Since moving up to the Kakute F7, I have only used Betaflight.

### Betaflight setup notes

* I used the CLI to remap the motors because I wired them completely out of order (hence the lack of detail in the wiring section above).
* You need to use the CLI to remap the servo 1 port to the pin `C09` (the former MOTOR 1 pin), so that Betaflight can drive the tail servo.  This is a difference from the Kakute F4.  With the F4, you had to use the LED port.
* Most configuration (ports, failsafe, receiver, motors, OSD, GPS rescue, etc) is exactly the same as a quad so I won't go into detail here.
* Servo 1 in the CLI maps to servo 5 in the configurator for some reason.  Use the configurator to set the mid so that the motor is exactly vertical, and set the min and max for 45° tilt.

### PIDs

I found that the default Betaflight PIDs don't work great for tricopters.  This is what I use (copied from somewhere in the RCExplorer forums):

|Axis   |P  |I  |D  |
|-------|---|---|---|
|Roll   |70 |25 |15 |
|Pitch  |70 |25 |15 |
|Yaw    |100|10 |0  |

No feed forward.  I Term Relax and Anti Gravity at the defaults.

Rate profiles are all .75 for rate, .70 for super rate, zero expo.  For throttle, 0.30 mid and 0.50 expo.  I like my tricopter tame.

### Config Files

A diff and full dump of my configs are in this repo.  **They are for reference only**.  Always set up your own config by hand.

## Links

* [Kakute F7 V1.5 manual](http://www.holybro.com/manual/Holybro_Kakute_F7_V1.5_Manual.pdf)
* [RCExplorer product page](https://rcexplorer.se/product/tricopter-lr/)
* [RCExplorer build video](https://www.youtube.com/watch?v=rNy6HMie_Yg)

## Changes

* Update to reflect using M5 for the servo signal instead of LED.