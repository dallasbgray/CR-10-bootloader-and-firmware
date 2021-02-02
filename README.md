# CR-10-Marlin

This is a guide I wrote for myself in case I ever need to flash a CR-10 bootloader or firmware again. It's not comprehensive and will assume you know quite a few things like how to install packages in Ubuntu or Debian, but it will offer guidance (cause it's a guide) so you'll know what to Google next. In addition most of this information is compiled from a number of videos, articles, and forum threads which I will link appropriately. 

This is ONLY for the CR-10 3D printer! The general process may apply to most 3D printers but any specific firmware, AVRISP, flashing method, is device specific! The instructions are for Windows 10, although it's probably easier on a Mac/Linux due to drivers being built in.

## Flashing the Bootloader

In order for 3D printer firmware to be updated, it must first have a bootloader. Newer printers with 32-bit boards usually come with that pre-installed so this entire section can be skipped. In the case with the CR-10, it has an 8-bit board and lacks a bootloader. 

    My CR-10 came with firmware version 1.1.0 on the Creality (Melzi)[https://www.reprap.org/wiki/Melzi#Can.27t_connect_to_the_Melzi_.28FTDI_driver_problem.29] v1.1.4 board. 

    (This Youtube Video)[https://www.youtube.com/watch?v=7J7NYnxL5vA] was very helpful for this process.

    You will need an Arduino Uno, 5 female=to=female gpio cables, 1 female-to-male gpio cable, and o coruse the USB cables to connect the Arduino Uno and the Melzi board.

    Unplug the printer and unscrew the bottom panel. Remove power supply but be careful due to the short wires. Just so long as we can see/access the main board inside. There will be 6 pins in a 2x3 arrangement at an edge of both the Arduino Uno board and the Melzi board. 

    **INSERT IMAGES HERE**

    MISO ----> MISO
    VCC --------> VCC
    SCK -----> SCK
    MOSI ---> MOSI
    RESET -> Digital Pin 10
    GND ----> GND   

    Attach the cables as shown below, each pin corresponds directly with the same pin on the other board except for the reset pin on the Melzi which plugs into digital pin 10 on the Uno. 

    **INSERT IMAGES HERE**

    After that, it's time to set up the Arduino IDE! I used v1.18.13 of the IDE. You'll need the right board drivers, for the Arduino IDE it is the (Sanguino)[https://github.com/Lauszus/sanguino] add-on for the 3D printer hardware. Just paste this link into the add-on board within the IDE preferences: https://raw.githubusercontent.com/Lauszus/Sanguino/master/package_lauszus_sanguino_index.json

    Try connecting the Melzi to your PC 

https://www.instructables.com/Flashing-a-Bootloader-to-the-CR-10/




https://hackaday.com/2014/10/24/ftdi-screws-up-backs-down/
https://hackaday.com/2014/10/24/ftdi-screws-up-backs-down/#comment-2059852


Link for the board driver from https://support.th3dstudio.com/hc/downloads/drivers/creality-printer-drivers-ft232r-chip-most-models/

but the actual driver is uploaded above as well. If it doesn't install, click 'uninstall' and then re-run the installer and install it again and it should succeed

## Updating the Firmware

https://www.youtube.com/watch?v=Ib188-ACa08
https://marlinfw.org/meta/download/
https://marlinfw.org/docs/hardware/boards.html

https://github.com/olikraus/U8glib_Arduino



Filename or extension too long https://github.com/olikraus/U8glib_Arduino/issues/9

PlatformIO https://github.com/olikraus/U8glib_Arduino/issues/9


Reducing space
https://github.com/MarlinFirmware/Marlin/issues/5216
https://crosslink.io/2020/08/14/shrinking-marlin-2-0-how-to-reduce-firmware-size-for-8-bit-boards-and-still-use-a-bltouch-and-the-filament-sensor/
