# CR-10-Marlin

---

This is a guide I wrote for myself in case I ever need to flash a CR-10 bootloader or firmware again. It's not comprehensive and will assume you know quite a few things like how to install packages in Ubuntu or Debian and that you have Octoprint configured, but it will offer guidance (cause it's a guide) so you'll know what to Google next! In addition most of this information is compiled from a number of videos, articles, and forum threads which I will link appropriately. 

This is ONLY for the CR-10 3D printer! The general process may apply to most 3D printers but any specific firmware, AVRISP, flashing method, is device specific! The instructions are for Windows 10, although it's probably easier on a Mac/Linux due to drivers being built in. 
    Note: I **do not recommend** downloading using the firmware I have uploaded here and instead strongly suggest you go to (Marlin's website)[https://marlinfw.org/meta/download/] in order get the latest firmware. This repository holds the firmware I used as my last known good configuration, so it's a personal 'backup' in a sense. Likely the only file you'll need is the CH340 driver if you are on Windows 10 for the Melzi board.

---

## Flashing the Bootloader

In order for 3D printer firmware to be updated, it must first have a bootloader. Newer printers with 32-bit boards usually come with that pre-installed so this entire section can be skipped. In the case with the CR-10, it has an 8-bit board (Melzi with the Atmel 1284p CPU) and lacks a bootloader. 

My CR-10 came with firmware version 1.1.0 on the Creality (Melzi)[https://www.reprap.org/wiki/Melzi#Can.27t_connect_to_the_Melzi_.28FTDI_driver_problem.29] v1.1.board. 

(This Youtube Video)[https://www.youtube.com/watch?v=7J7NYnxL5vA] was very helpful for figuring out this process.

 You will need an Arduino Uno, 5 female=to=female gpio cables, 1 female-to-male gpio cable, and of coruse the USB cables to connect the Arduino Uno and the Melzi board.

Unplug the printer and unscrew the bottom panel. Remove power supply but be careful due to the short wires. Just so long as we can see/access the main board inside. There will be 6 pins in a 2x3 arrangement at an edge of both the Arduino Uno board and the Melzi board. 

 **INSERT IMAGES HERE LATER**

Attach the cables as shown below, each pin corresponds directly with the same pin on the other board except for the reset pin on the Melzi which plugs into digital pin 10 on the Uno. 

Melzi ---> Arduino Uno
MISO ----> MISO
VCC -----> VCC
SCK -----> SCK
MOSI ----> MOSI
RESET ---> Digital Pin 10
GND -----> GND   

**INSERT IMAGES HERE LATER**

If you are on a version of Melzi before v1.1.4 there is probably a little switch on it that lets you switch from the PSU power to USB power, so go ahead and move the switch so it will be powered via the USB cable you had set aside from earlier.

After that, it's time to set up the (Arduino IDE)[https://www.arduino.cc/en/software]! I used v1.18.13. You'll need the right board drivers, for the Melzi board get the the (Sanguino)[https://github.com/Lauszus/sanguino] add-on for the 3D printer hardware. Just paste this link into the add-on board within the IDE preferences: https://raw.githubusercontent.com/Lauszus/Sanguino/master/package_lauszus_sanguino_index.json

 Check which COM port the Uno and Melzi appear on by plugging them in. Windows 10 may need a driver if it doesn't show up in the COM ports in Device Manager. Download the driver I got (here from TH3D){https://support.th3dstudio.com/hc/downloads/drivers/ch340-drivers-th3d-uno-creality-v1-1-x-v4-2-x-board/}, but you can also download the copy above (CH341SER.exe is the CH430 driver). If the install fails, click 'uninstall' and then re-run the installer again it should succeed. Look up TH3D firmware to give it a shot, they seem to have some popularity due to ease of use and installation of their firmware.

Both board should power on when either one is plugged in.

Connect the Uno to the IDE and select the right COM port. Now we're going to go to File -> Examples -> ArduinoISP -> Select the ArduinoISP file and upload it to the Uno (of course make sure the settings are similar to the image below).

**INSERT IMAGES HERE LATER**





Now it's time to upload!


https://www.instructables.com/Flashing-a-Bootloader-to-the-CR-10/




https://hackaday.com/2014/10/24/ftdi-screws-up-backs-down/
https://hackaday.com/2014/10/24/ftdi-screws-up-backs-down/#comment-2059852


Link for the board driver from https://support.th3dstudio.com/hc/downloads/drivers/creality-printer-drivers-ft232r-chip-most-models/

---

## Updating the Firmware

(to be added at a later date)
