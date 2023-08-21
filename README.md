# MBC1_DIP_Flash
Game Boy MBC1 Flash Cart with swappable DIP flash IC

![Populated board 3D render](https://github.com/ConsolesandCasks/MBC1_DIP_Flash/blob/main/MBC1_DIP_flash2.png)

**License: CC BY-SA 4.0** - https://creativecommons.org/licenses/by-sa/4.0/

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

I've also placed a 100nF decoupling capacitor at +5V on the flash IC. This should be close enough to the MBC1 as well to serve a dual purpose, but if it seems like noise is an issue I may add one for it specifically in the future. If anyone wants to do math to figure out what the value should _actually_ be - go for it (just let me know).

## BOM

| Reference        | Part Number*           | Description  | Package | Link* |
| --- |-------------| -----| -----|------|
| U1 | MBC1A/1B/1B1 | Memory Bank Controller | SOIC-24/SOP-24 (450mil) | [Harvested](https://catskull.net/gb-rom-database/)| 
| U2 | SST39SF040/A29040/AM29F040 | 512KB NOR Flash | DIP-32/PDIP-32 | [Mouser](https://www.mouser.com/ProductDetail/Microchip-Technology/SST39SF040-70-4C-PHE?qs=YClUa%252B2dcx1pgizrqJ6nyQ%3D%3D) | 
| U2* | DILB32P-223TLF | 32 pin DIP socket | DIP-32/PDIP-32 | [Mouser](https://www.mouser.com/ProductDetail/Amphenol-FCI/DILB32P-223TLF?qs=dNsYR%2FH0PyPAfvGxulPprw%3D%3D) |
| C1 | 06033C104KAT4A | 0.1uF MLCC | 0603 SMD | [Mouser](https://www.mouser.com/ProductDetail/KYOCERA-AVX/06033C104KAT4A?qs=8C2chATdSPiv3E9zfPZulg%3D%3D) |
| R1 | RC0603FR-0710KL | 10kOhm Thick Film Resistor | 0603 SMD | [Mouser](https://www.mouser.com/ProductDetail/YAGEO/RC0603FR-0710KL?qs=grNVn54RoB%252B3GtjbJj3wJQ%3D%3D) |

*suitable alternative parts and sources exist for all components other than MBC 

## Compatibility/Testing

### Flash IC
**Any 32-pin 512KiB (4mbit) parallel DIP flash is _likely_ compatible.**
I have only tested with SST39SF040-70-4C-PH and A29040 (Aug 18 2023, Rev 1.1) but, in theory, this should also be compatible with an SST39SF040, A29040, AM29F040, W29C040, as well as SST39SF020, SST39SF010, 27C020 (eeprom), and 27C010 (eeprom) and many more as long as it is in this same package. If you're unsure, compare the pinouts. _You will be unable to write to an eeprom variant_ 

NOTE: I have not tested _any_ EEPROMS (and 512KiB EEPROMs are 100% incompatible without significant modification - cut the trace to pin 31 and pin 1 on the DIP and run a wire from pin 14 on the MBC to pin 31 on the flash). Most EEPROMs will also need to be written to externally with something like a universal programmer, rather than through a cart writer device.

### Solder bridges
The jumpers under the MBC1 are for MBC-less operation. _ONLY bridge these points if you do not plan to install an MBC1._ With the bridges soldered the board will operate in 32k mode for any game of that size that does not require an MBC or saves.

### ROM support
With MBC it should support any GB rom that is either **ROM only, or MBC1+ROM only up to 512KiB**, there is **NO SAVE FUNCTION ON THIS BOARD**

Without MBC (w/ solder bridges jumpered) it will only support **ROM only** games.

You can check compatibility here: https://catskull.net/gb-rom-database/ - if you run into any issues with compatibility that contradicts the above, please let me know

### Writing to the Flash IC
My testing utilized a GBxCart 1.4 Pro and FlashGBX to write. It will autodetect as "DIY cart with SST39SF040 @ AUDIO" (or other supported DIY profile for other chips) There is no profile for A29040 but writing with the SST39SF040 @ AUDIO profile will work. For other chips, use the auto detected profile, or try either the SST profile or generic @ audio write profile.

GBx does not provide the necessary voltage to write to EEPROM ICs, so you will need a separate writer for such chips

I have tested 144p test suite, orangeglo's better button test, Super Mario Land, and MegaMan V so far on the Rev 1.0 boards (Aug 18, 2023) in both MBC1 and no MBC configurations (for 144p test and bb test specifically).

### How to get
I would recommend [JLCPCB](https://jlcpcb.com/) for the least expensive options for having boards fabricated

Gerber silkscreen is set up for JLC to "specify a location" for the order number (seperate gerber files will be added if you choose to remove the order number or use a different PCB fabricator service

Options should be set to 0.8 thickness, ENIG surface finish, Gold fingers, 30 degree chamfer


I have a number of purple Rev 1.1 boards produced (Aug 18, 2023) - and I am currently looking into a way to distribute them.
![MBC1-DIP-FLASH-Built.jpg](https://github.com/ConsolesandCasks/MBC1_DIP_Flash/blob/main/MBC1-DIP-FLASH-Built.jpg)

## Notes on shell fitment
The through-hole pins on the back of the DIP connector will likely need to be trimmed so the board can sit somewhat flat against a shell back. Most of my testing has been simply with the board resting against a shell back inserted into GBxCart or a Game Boy.
I have tested these with the 3 standard single-screw style OEM shells and Kitsch-Bent shell *backs*.

For a more permanent shell, "Half-shell" Kitsch-Bent shells are likely ideal. [KB Referral Link](https://kitsch-bent.refr.cc/consolescasks) *NOTE: I have not tested these yet, but because of the size and location of the cutout, I'm pretty confident on proper fit. 

To use an OEM shell (or aftermarket like CGS) front, you need to cut the front of the shell approximately 1/3 of the way down (slightly below where the label begins and just above where the pin on the back rests on a DMG shell; On a GBC cart it should be just barely into the label cutout). You should also be able to cut a *slot* the width of the Game Boy Logo grip (down to the same point).


## Changelog
**Rev 1.1**
Properly routed 1.0 bodge wires

Added solder jumper bridges for MBC-less operation

Additional rerouting for better grounding and aesthetics

**Rev 1.0**
Initial design
