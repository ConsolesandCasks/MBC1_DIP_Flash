# MBC1_DIP_Flash
Game Boy MBC1 Flash Cart with swappable DIP flash IC (Rev 1.1 is **UNTESTED** as of Aug. 7, 2023 - on order for testing)

**License: CC BY-NC-SA 4.0** - https://creativecommons.org/licenses/by-nc-sa/4.0/

This uses djedditt's footprints and outlines: https://github.com/djedditt/kicad-gamepaks

Inspired by SillyHatDay's Discrete MBC1 EEPROM Cartridge: https://github.com/sillyhatday/GAMEBOY-FLASHCART-MBC1-DISCRETE

## Summary:
The general concept here is an MBC1 flash cart with a swappable ROM chip that is actually flash (opposed to previous iterations that were EEPROMs). I'm using a harvested MBC1 here for simplicity sake.


The pinout of a 27C040 matches a SST39SF040-70-4C-PHE except for 2 pins (WE and A18). This cart connects WE to the AUD pin (31) on the Game Boy cartridge connector (which is a commonly used write enable pin for flash cartridges, and avoids potential issues with using pin 3).

## Schematic
I apologize for the messy/unprofessional looking schematic, this was my first KiCad project and I anticpated less net routing than there actually was. I may eventually clean this up, but as of Rev 1.1 it is a bit of a rats nest because it was quicker for me (given the bulk of my schematic experience is in MultiSim) to just connect the wires than properly net everything.

I referenced very old schematics and pin-outs located at http://www.devrs.com/gb/files/hardware.html and https://wiki.tauwasser.eu/view/MBC1

### Passives
There is a 10k pull up resistor on the AUD (flash write) net to prevent errant input to the Game Boy. This should be sufficient but if you're running into issues with your specific flash IC you may need to increase or decrease this value.

I've also placed a 100nF decoupling capacitor at +5V on the flash IC. This may not be necessary but I put it in as a precaution. This should be close enough to the MBC1 as well to serve a dual purpose, but if it seems like noise is an issue I may add one for it specifically in the future. If anyone wants to do math to figure out what the value should _actually_ be - go for it (just let me know).

## Compatibility/Testing

### Flash IC
**Any 32-pin 512KiB (4mbit) parallel DIP flash is _likely_ compatible.**
I have only tested with SST39SF040-70-4C-PH and AM29040 (Aug 7 2023, Rev 1.0) but, in theory, this should also be compatible with an SST39SF040, A29040, AM29F040, W29C040, as well as SST39SF020, SST39SF010, 27C020 (eeprom), and 27C010 (eeprom) and many more as long as it is in this same package. If you're unsure, compare the pinouts. _You will be unable to write to an eeprom variant_ 

NOTE: I have not tested _any_ EEPROMS and 512KiB EEPROMs are 100% incompatible without significant modification (cut the trace to pin 31 and pin 1 on the DIP and run a wire from pin 14 on the MBC to pin 31 on the flash) 

### Solder bridges
The jumpers under the MBC1 are for MBC-less operation. _ONLY bridge these points if you do not plan to install an MBC1._ With the bridges soldered the board will operate in 32k mode for any game that does not require an MBC or saves

### ROM support
With MBC it should support any GB rom that is either **ROM only, or MBC1+ROM only up to 512KiB**, there is **NO SAVE FUNCTION ON THIS BOARD**

Without MBC (w/ solder bridges jumpered) it will only support **ROM only** games.

You can check compatibility here: https://catskull.net/gb-rom-database/ - if you run into any issues with compatibility that contradicts the above, please let me know

### Writing to the Flash IC
My testing utilized a GBxCart 1.4 Pro and FlashGBX to write. It will autodetect as "DIY cart with SST39SF040 @ AUDIO" (or other supported DIY profile for other chips) There is no profile for A29040 but writing with the SST39SF040 @ AUDIO profile will work.

I have tested 144p test suite, Super Mario Land, and MegaMan V so far on the Rev 1.0 boards (Aug 7 2023) using bodge wires in both MBC1 and no MBC configurations.



## Changelog
**Rev 1.1**
Properly routed 1.0 bodge wires
Added solder jumper bridges for MBC-less operation
Additional rerouting for better grounding and aesthetics

**Rev 1.0**
Initial design
