# KiCad Model of Logical Devices' ALLPRO88 Pin Driver

## Overview

This provides a KiCad [schematic](https://github.com/user-attachments/files/18313981/pin_driver.pdf) of the pin driver used in Logic Devices' ALLPRO88 software-driven device programmer.  The service manual scans available on the internet are missing the pages containing the pin driver circuitry.  I believe the schematic is correct, it's certainly correct enough for me to have repaired many of these modules based on what's here, but I found errors as recently as the fall of 2024 so I wouldn't say it's definitely correct.  Your mileage might vary.

There is no PCB layout.  The PCBs from Logic Devices have enough component designators marked in the silk screen to find parts and associate them with the schematic.

See also https://github.com/cannon-zz/allpro88-pin-driver-hybrid.

## Service suggestions

Failures I've seen:

 * D1 through D8 (1N5815 Schottky diodes used for reverse protection on VDAC output drivers) frequently fail, becoming resistor-like when reverse biased.  I have taken to replacing all of them, on principle, whenever I have a pin driver out on the bench.  When they fail, they provide a path to ground through the negative current source used to bias the LM395P's into an emitter-follower configuration.  Possibly the most clear symptom of this fault is a low output voltage on the pull-up source:  enable pull-up and pull-down drivers simultaneously, then ramp the pull-up source from 0 to max/2, measuring the voltage on the pin with the voltage read-back feature;  if you see a nice linear ramp, but the slope is less than other pins, suspect the VDAC reverse protection diode.

 * U15, U16:  x244 bus buffers;  outputs must equal inputs, always.  I saw a U15 (data bus buffer) stuck once, with all output bits high at all times.  That caused all output drivers on that module to be enabled, and the DAC drivers set to max, destroying the Schottky diodes as above and making the heatsink quite hot.

 * U12, U13:  LM346N quad op-amp current-to-voltage converters for VDAC drivers.  I've never seen these fail, but several of my boards show evidence of these chips having been replaced, including one that got socketed suggesting a repeat offender.  The schematic contains a note explaining what voltage to look for on their outputs given the DAC setting.  Check the full output swing.
