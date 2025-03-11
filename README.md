# 3d-printing-nuggets
Nuggets of information mined via hours of frustration

## Mini 12864 pitfalls
mini 12864 displays from different manufacturers can have different EXP1/EXP2 header layouts. If your new display doesn't work, try putting the connector in backwards. You can do this by grinding the key off the connector or by pulling the plastic surround off the display PCB, rotating it around, and putting it back on backwards. The notch will now be on the other side. 

## BTT "FPC" connector
When BTT mentions an "FPC" connector, it most likely refers to an 18-pin, 1-mm pitch connector. 

## Install Linux on CB1/CB2 emmc from microSD card
The easy way to install Linux on a BTT CB1/2 board is to:
1. Write the image to a microSD card.
2. Power the printer up with the microSD card in the microSD slot.
3. SSH into the printer (root/root or biqu/biqu depending on OS image).
4. Run the nand-sata-install command as root.
If you have multiple printers to set up, run KIAUH while booted from the microSD card.

## Build Katapult or Klipper for Cartographer
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

## Get the klipper-mcu service running on a CB2 with the Armbian bookworm release
BTT has some issues with its Arbian bookworm-based release. Here is the quick and dirty way to get the klipper-mcu serice working from scratch. 
```
cd ~/klipper/
make menuconfig
```
Select Linux process
```
sudo cp ./scripts/klipper.service /etc/systemd/system/
echo 'ExecStartPre=/bin/sleep 10' | sudo tee -a /etc/systemd/system/klipper-mcu.service
sudo systemctl enable klipper-mcu.service
sudo service klipper stop
make flash
sudo sysctl -w kernel.sched_rt_runtime_us=-1
echo "kernel.sched_rt_runtime_us = -1" | sudo tee /etc/sysctl.d/10-disable-rt-group-limit.conf
sudo service klipper start
```
This will build the klipper-mcu binary, install the service, and make two tweaks to make it work on BTT's OS release. The line about `kernel.sched.rt_runtime_us` is the critical tweak. The line with `ExecStartPre=/bin/sleep 10` causes Klipper to wait a few seconds to start so that the klipper-mcu service can do its work first. 
