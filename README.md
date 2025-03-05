# 3d-printing-nuggets
Nuggets of information mined via hours of frustration

mini 12864 displays from different manufacturers can have different EXP1/EXP2 header layouts. If your new display doesn't work, try putting the connector in backwards. You can do this by grinding the key off the connector or by pulling the plastic surround off the display PCB, rotating it around, and putting it back on backwards. The notch will now be on the other side. 

When BTT mentions an "FPC" connector, it most likely refers to an 18-pin, 1-mm pitch connector. 

The easy way to install Linux on a BTT CB1/2 board is to:
1. Write the image to a microSD card.
2. Power the printer up with the microSD card in the microSD slot.
3. SSH into the printer (root/root or biqu/biqu depending on OS image).
4. Run the nand-sata-install command as root.
If you have multiple printers to set up, run KIAUH while booted from the microSD card.

If you need to built Katapult or Klipper for a Cartographer v3 for Canbus, use these settings in make menuconfig:
* MCU arch: STM32
* Processor model: STM32F042
* Build Katapult deploy app (8 KiB)
* Clock reference 24 MHz crystal
* Comm interface Canbus on PA9/PA10
* App start offset 8 KiB
* 1000000 Canbus speed (or your Canbus speed of choice)
* (PA1) GPIO pins to set on bootloader entry
* Support bootloader entry on double reset
* (PB5) Status LED GPIO pin
