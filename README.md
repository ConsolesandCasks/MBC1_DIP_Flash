# MBC1_DIP_Flash
Game Boy MBC1 Flash Cart with swappable DIP flash IC

License: CC BY-NC-SA 4.0 - https://creativecommons.org/licenses/by-nc-sa/4.0/

This uses djedditt's footprints and outlines https://github.com/djedditt/kicad-gamepaks

Inspired by SillyHatDay's Discrete MBC1 EEPROM Cartridge: https://github.com/sillyhatday/GAMEBOY-FLASHCART-MBC1-DISCRETE

The general concept here is an MBC1 flash cart with a swappable ROM chip that is actually flash (opposed to previous iterations that were EEPROMs). I'm using a harvested MBC1 here for simplicity sake.

The pinout of a 27C040 matches a SST39SF040-70-4C-PHE except for 2 pins (WE and A18). This cart connects WE to the AUD pin on the Game Boy cartridge connector (which is the commonly used write enable pin for flash cartridges).

In theory this should also be compatible with a A29040, SST39SF020, SST39SF010, 27C020, and 27C010 of the same package type (32-pin DIP), as well as any other 512 kilobyte or lower parallel flash or 256 kilobyteor lower EEPROM with the same pinout, although it will be unable to write to the EEPROM variants.

It should also support any GB rom that is either ROM only, or MBC1+ROM only.
