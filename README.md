# CR-10-Marlin

---

This is a guide I wrote for myself in case I ever need to flash a CR-10 bootloader or firmware again. It's not comprehensive and will assume you know quite a few things like how to install packages in Ubuntu or Debian and that you have Octoprint configured, but it will offer guidance (cause it's a guide) so you'll know what to Google next! In addition most of this information is compiled from a number of videos, articles, and forum threads which I will link appropriately. 

This is ONLY for the CR-10 3D printer! The general process may apply to most 3D printers but any specific firmware, AVRISP, flashing method, is device specific! The instructions are for Windows 10, although it's probably easier on a Mac/Linux due to drivers being built in. 
    Note: I **do not recommend** \using the firmware I have uploaded here and instead strongly suggest you go to [Marlin's website](1) in order get the latest firmware. This repository holds the firmware I used as my last known good configuration, so it's a personal 'backup' in a sense. Likely the only file you'll need is the CH340 driver if you are on Windows 10 for the Melzi board.

---

## Flashing the Bootloader

In order for 3D printer firmware to be updated, it must first have a bootloader. Newer printers with 32-bit boards usually come with that pre-installed so this entire section can be skipped. In the case with the CR-10, it has an 8-bit board (Melzi with the Atmel 1284p CPU) and lacks a bootloader. 

[This YouTube video](2) was very helpful for figuring out this process.

My CR-10 came with firmware version 1.1.0 on the Creality [Melzi](3) v1.1.board. 

You will need:
- of course a CR-10 3D printer
- An Arduino Uno
- 5 female=to=female gpio cables
- 1 female-to-male gpio cable
- USB cables for the Arduino Uno and the Melzi.

Connect the Uno to the IDE and select the right COM port. Now we're going to go to File -> Examples -> ArduinoISP -> Select the ArduinoISP file and upload it to the Uno (of course make sure to select the Arduino Uno in the boards manager).

Unplug the printer and unscrew the bottom panel. Remove power supply but be careful due to the short wires. Just so long as we can see/access the main board inside. There will be 6 pins in a 2x3 arrangement at an edge of both the Arduino Uno board and the Melzi board. 

 **IMAGES COMING SOON**

Attach the cables as shown below, each pin corresponds directly with the same pin on the other board except for the reset pin on the Melzi which plugs into digital pin 10 on the Uno. The reset pin on the Uno will remain unconnected.

Melzi ---> Arduino Uno
MISO ----> MISO
VCC -----> VCC
SCK -----> SCK
MOSI ----> MOSI
RESET ---> Digital Pin 10
GND -----> GND   

**IMAGES COMING SOON**

If you are on a version of Melzi before v1.1.4 there is probably a little switch on it that lets you switch from the PSU power to USB power, so go ahead and move the switch so it will be powered via the USB cable you had set aside from earlier.

WARNING: double check these conenctions are correct!

After that, it's time to set up the [Arduino IDE](5)! I used v1.18.13. You'll need the right board drivers, for the Melzi board get the the [Sanguino](6) add-on for the 3D printer hardware. Just paste this link into the add-on board within the IDE preferences: https://raw.githubusercontent.com/Lauszus/Sanguino/master/package_lauszus_sanguino_index.json

 Check which COM port the Uno and Melzi appear on by plugging them in. Windows 10 may need a driver if it doesn't show up in the COM ports in Device Manager. Download the driver I got (here from TH3D){https://support.th3dstudio.com/hc/downloads/drivers/ch340-drivers-th3d-uno-creality-v1-1-x-v4-2-x-board/}, but you can also download the copy above (CH341SER.exe is the CH430 driver). If the install fails, click 'uninstall' and then re-run the installer again it should succeed. Look up TH3D firmware to give it a shot, they seem to have some popularity due to ease of use and installation of their firmware.

Both boards should power on when either one is plugged in.

If this process fails you will have erased the firmware on the board and will not have a bootloader to load firmware over USB so you would have to burn a bootloader successfully in order to get firmware back on the board.

To burn the bootloader plug in the Arduino Uno, go to Tools and select:
- Sanguino as the board
- Processor as the ATmega1248p (16MHz)
- Port as the Arduino Uno (NOT the Sanguino/Melzi. I know I know, it's a lot of names for the same thing, but the ISP is an In System Programmer not your Internet Service Provider, so in simple terms we're using the Uno as an interface to program the Melzi)
- Programmer as Arduino as ISP (very important to select "Arduino as ISP" and not "ArduinoISP" or the other alternatives) 
- Select burn bootloader

**IMAGES COMING SOON**

It should be done in a few seconds and there you have it, the bootloader has been flashed to your board once and for all! Now you can update the firmware whenever you'd like!!!

There's currently no firmware on the board now, so check out the steps below to get that started!

Referenes: This [Instructable article](8) was very informative as well as the [YouTube video](2) mentioned above

If you are curious about the issue with FDTI chips that makes the Melzi sometimes not appear on the COM ports check out this [FTDI driver problem](4) and look at the threads which discuss the ripoff chips that drivers tend to have issues with. In theory it was fixed, but even with Windows v2004 I still had a problem with this. I solved it with the [driver from TH3D](7) so if you have issues try this out. For other board versions look at this [TH3D webpage](9).

---

## Updating the Firmware

To be added at a later date :)






[1]: https://marlinfw.org/meta/download/ "Marlin Firmware"
[2]: https://www.youtube.com/watch?v=7J7NYnxL5vA "Creality CR-10 Marlin Firmware Upgrade-Chris's Basement"
[3]: https://www.reprap.org/wiki/Melzi#Introduction "Melzi documentation"
[4]: https://www.reprap.org/wiki/Melzi#Can.27t_connect_to_the_Melzi_.28FTDI_driver_problem.29 "Melzi problem/workaround documentation on RepRap"
[5]: https://www.arduino.cc/en/software "Arduino Software Downloads"
[6]: https://github.com/Lauszus/sanguino
[7]: https://support.th3dstudio.com/hc/downloads/drivers/ch340-drivers-th3d-uno-creality-v1-1-x-v4-2-x-board/ "board v1.1.4 CH340 driver download"
[8]: https://www.instructables.com/Flashing-a-Bootloader-to-the-CR-10/ "Instructables Flashing a Bootloader to the CR-10"
[9]: https://support.th3dstudio.com/hc/downloads/drivers/creality-printer-drivers-ft232r-chip-most-models/ "other board verison driver downloads"
[10]:
[11]:
[12]:
[13]:
[14]:
[15]:
[16]:
[17]:
[18]: