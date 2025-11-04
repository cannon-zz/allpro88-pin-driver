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

 * At one time I purchased a set of boards off eBay to complete the upgrade of my two 48 channel units to the full 88 channels, each, and in 6 of the boards, not a single one of the LM395P VDAC output power transistors --- nearly 50 transistors in all --- had been properly screwed to the heat sinks.  The little plastic insulators used to isolate the transistors and their bolts from electrically contacting the heat sinks, instead of having been inserted into the bolt holes they had just been crushed in behind the transistors creating gaps between the transistors and the heatsinks.  The LM395P datasheet claims they're basically indestructible, they have over-temp protection so not being cooled won't damage them but it might damage the part you're programming if its supply voltages sag because the VDAC output transistors get too hot and throttle back.  Given how many of my own boards I've had to repair, I wasn't expecting the eBay units to work out of the box, but when I saw the specific failure I became curious about their history:  were they assembly line rejects that had been bought in a liquidation sale years ago?  The seller said they'd been pulled from a working system that had never been opened since being purchased decades ago.  The boards came from the factory that way!  I would say it's worth taking a look at the boards in your unit and reinstalling the transistors if needed.  The crushed isolators in my boards could not be pushed back into shape and salvaged, but were also hard to replace.  I believe the correct part is Fischer Elektronik IB 19, or equivalent.  One of the dimensions is unusual, but I don't remember which.  I was not able to get equivalents, I found some generic TO-220 isolators on Amazon that were close then softened them with heat and used persuasion to make them fit.

 * In that same group of 6 boards, on every board there was at least one pin on a component somewhere that hadn't been soldered, and on some many pins had not been soldered.  In the case of one board, an entire capacitor hadn't even been installed.  I had to basically finish building them.  Check your boards.  Definitely check your boards.
