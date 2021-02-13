# CR-10 Bootloader and Firmware Guide

[//]: # (just experimenting with hiding comments ;)

This is a guide I wrote for myself in case I ever need to flash a CR-10 bootloader or firmware again. It's not comprehensive and will assume you know quite a few things like how to install packages in Ubuntu or Debian and that you are familiar with the Arduino IDE and Octoprint, but it will offer guidance so you'll know what to Google next! Most of this information is compiled from a number of videos, articles, and forum threads which I will link appropriately. 

This is ONLY for the CR-10 3D printer! The general process may apply to most 3D printers but any specific firmware, AVRISP, flashing method, is device specific! The instructions are for Windows 10, although it's probably easier on a Mac/Linux due to drivers being built in. 
    
>Note: I ***strongly advise against*** using the firmware I have uploaded here and instead suggest you go to [Marlin's website](https://marlinfw.org/meta/download/ "Marlin Firmware") in order get the latest firmware. I modified the firmware above to **remove several features** in order to fit it on the 8-bit board, and this is just me saving my last known good config. Check **below** to see what I changed.

---

# Flashing the Bootloader

In order for 3D printer firmware to be updated, it must first have a bootloader. Newer printers with 32-bit boards usually come with that pre-installed so this entire section can be skipped. In the case with the CR-10, it has an 8-bit board (Melzi with the ATmega1284p) and lacks a bootloader so we've got to add one. 

[This YouTube video](https://www.youtube.com/watch?v=7J7NYnxL5vA "Creality CR-10 Marlin Firmware Upgrade-Chris's Basement") was very helpful for figuring out this process.


| AVR MCU 		|	Programmer	 | 	Example Board	|
| :------------ | :------------- | :----------------- |
| Atmega1284p 	|	arduino 	 |	Anet A series, Creality, Ender, etc. (CR-10 has this one)  |
| Atmega2560 	|	wiring 		 |	RAMPS, RAMbo, etc.		|
| Atmega644p 	|	arduino 	 |	Sanguinololu, Melzi		|
| Atmega32u4 	|	avr109 		 |	Prusa MMU, Prusa CW1	|

>My CR-10 came with firmware version 1.1.0 on the Creality [Melzi](https://www.reprap.org/wiki/Melzi#Introduction "Melzi documentation") v1.1.4 board with the ATmega1284p microcontroller.

## Parts List & Wiring

You will need:
- 1 Arduino Uno
- 5 female-to-female gpio cables
- 1 female-to-male gpio cable
- USB cables for the Arduino Uno and the Melzi.

Connect the Uno to the IDE and select the right COM port. Now we're going to go to 
>File
>>Examples
>>>ArduinoISP
>>>>Select the ArduinoISP file and upload it to the Uno (of course make sure to select the Arduino Uno in the boards manager).

Unplug the printer and unscrew the bottom panel. Remove power supply but be careful due to the short wires. Just so long as we can see/access the main board inside. There will be 6 pins in a 2x3 arrangement at an edge of both the Arduino Uno board and the Melzi board. 

<p align="center">
<img src="/img/melzi_pinout.jpg" width=400 alt="Figure 1: Melzi Pinout">
</p>

Attach the cables as shown below, each pin corresponds directly with the same pin on the other board except for the reset pin on the Melzi which plugs into digital pin 10 on the Uno. The reset pin on the Uno will remain unconnected.

#### Melzi to Arduino Uno Wiring
- MISO ----> MISO
- VCC -----> VCC
- SCK -----> SCK
- MOSI ----> MOSI
- RESET ---> Digital Pin 10
- GND -----> GND

<p align="center">
<img src="/img/completed_wiring.jpg" width=400 alt="Figure 2: Completed Wiring">
</p>

If you are on a version of Melzi before v1.1.4 there is a little switch on it that lets you switch from the PSU power to USB power, so go ahead and flip the switch so it will be powered via the USB cable you had set aside from earlier.

>**WARNING**: double check these conenctions are correct! You don't want to fry anything.

Both boards should power on when either one is plugged in.

### Arduino IDE setup
After that, it's time to set up the [Arduino IDE](https://www.arduino.cc/en/software "Arduino Software Downloads")! I used v1.18.13. You'll need the right board drivers, for the Melzi board get the the [Sanguino](https://github.com/Lauszus/sanguino) add-on for the 3D printer hardware. Just paste this link into the add-on board within the IDE preferences: ```https://raw.githubusercontent.com/Lauszus/Sanguino/master/package_lauszus_sanguino_index.json```

>Check which COM port the Uno and Melzi appear on by plugging them in. Windows 10 may need a driver if it doesn't show up in the COM ports in Device Manager. Download the driver I got [here from TH3D](https://support.th3dstudio.com/hc/downloads/drivers/ch340-drivers-th3d-uno-creality-v1-1-x-v4-2-x-board/), but you can also download the copy above (CH341SER.exe is the CH430 driver). Here is an [alternate CH340 driver download](https://sparks.gogo.co.nz/ch340.html). If the install fails, click 'uninstall' and then re-run the installer again it should succeed. Look up TH3D firmware to give it a shot, they seem to have some popularity due to ease of use and installation of their firmware.

If this process fails you will have erased the firmware on the board and will not have a bootloader to load firmware back onto the board again. You do need to burn a bootloader successfully in order to upload any firmware back on the board, but if your wiring is correct and the Arduino IDE Tools options are correct you should have no issues.

### Burning the Bootloader

#### To burn the bootloader plug in the Arduino Uno, go to Tools and select:
1. Sanguino as the board
2. Processor as the ATmega1248p (16MHz)
3. COM Port as the Arduino Uno (NOT the Melzi/Sanguino)
4. Programmer as Arduino as ISP (very important to select "Arduino as ISP" and not "ArduinoISP" or the other alternatives) 
5. Select burn bootloader

If you're not sure it should look like this, but with the COM port for the Uno selected:

<p align="center">
<img src="/img/arduino_isp_settings.png" width=400 alt="Figure 3: Arduino IDE Tools Menu">
</p>

It should be done in a few seconds and there you have it, the bootloader has been flashed to your board once and for all! Now you can update the firmware whenever you'd like!!!

<p align="center">
<img src="/img/marlin_splashscreen.jpg" width=400 alt="Figure 4: Marlin Splashscreen">
</p>

There's currently no firmware on the board now and you'll get a blank screen, so check out the steps below to add the firmware!

References: This [Instructable article](https://www.instructables.com/Flashing-a-Bootloader-to-the-CR-10/ "Instructables Flashing a Bootloader to the CR-10") was very informative as well as the [YouTube video](2) mentioned above

If you are curious about the issue with FDTI chips that makes the Melzi sometimes not appear on the COM ports check out this [FTDI driver problem](https://www.reprap.org/wiki/Melzi#Can.27t_connect_to_the_Melzi_.28FTDI_driver_problem.29 "Melzi problem/workaround documentation on RepRap") and look at the threads which discuss the ripoff chips that drivers tend to have issues with. In theory it was fixed, but even with Windows v2004 I still had a problem with this. I solved it with the [driver from TH3D](https://support.th3dstudio.com/hc/downloads/drivers/ch340-drivers-th3d-uno-creality-v1-1-x-v4-2-x-board/ "board v1.1.4 CH340 driver download") so if you have issues try this out. For other board versions look at this [TH3D webpage](https://support.th3dstudio.com/hc/downloads/drivers/creality-printer-drivers-ft232r-chip-most-models/ "other board version driver downloads").

---

# Flash the Firmware

If your board already had a bootloader or you just flashed one, now's the time to update the firmware! The CR-10 (our printer) uses Marlin, so download the latest stable release of the [Marlin Firmware](https://marlinfw.org/meta/download/ "Marlin Firmware"). If you're not sure check out this [Marlin documentation](https://marlinfw.org/docs/hardware/boards.html).

The configuration files don't come by default in the firmware anymore and are on a separate download from the webpage.
Follow this path:
> config
>> examples
>>> Creality
>>>> CR-10
>>>>> CrealityV1

Configuration.h and Configuration_adv.h are needed for Marlin to configure the printer properly, and _Bootscreen.h and _Statusscreen.h are if your 3D printer has an LCD to use. 

Copy those files and past them in the "Marlin" directory of the firmware located here (yes, please select *replace files in destination*):
> Marlin-2.0.x
>> Marlin

[This other YouTube video](https://www.youtube.com/watch?v=Ib188-ACa08 ) was very helpful for figuring out this process.
 
There are a few ways to do this, I will list three here.

### 1. Flash Using the Arduino IDE

Go into your ```Marlin``` firmware folder and open ```Configuration.h``` in the Arduino IDE.
Check the Tools menu and be sure to have the Sanguino board selected (steps for setting up the Arduino IDE are above) **EDIT HERE**

<img src="/img/arduino_isp_settings.png" width=400 alt="Figure 5: Arduino IDE Tools Menu">

Select **upload** and you're all set, the firmware has been uploaded to the board! Put it back together, and you're all ready to start printing again.

Errors I ran into:

> - *Header files not found for U8glib*: This means you're missing the [U8glib_Arduino library](https://github.com/olikraus/U8glib_Arduino) used for the LCD. Just add the .zip library to the IDE and reupload.
> - *Filename or extension too long*: I hit Arduino IDE issues dealing with filenames and file path lengths being too long which I couldn't resolve, so I used VSCode with PlatformIO instead. More info in [this thread](https://github.com/olikraus/U8glib_Arduino/issues/9 "Filename or extension too long"), but you may be able to figure this out.

### 2. Flash using Visual Studio with PlatformIO

Download Visual Studio Code, and get the PlatformIO plugin from the marketplace. 

Open the Marlin-2.0.x folder in VSCode and you should see the folder open and with your configuration files in the file explorer.

>The flashing process works for both 8-bit and 32-bit boards.

Here's the important part: 
1. select the PlatformIO extension icon on the left toolbar
2. under *Project Tasks* select env:sanguino1284p
3. Select *Upload* to build and upload to the Melzi board.

If you get a problem, try comparing the device COM port with the COM port VSCode tried to upload to. It does autodetect the COM port, but it can be fixed usually by restarting VSCode. 

Errors I ran into:

> - *The board doesn't have enough memory*: It's an 8-bit board so it only has 128K memory, very little by modern standards! A good rule of thumb is that the firmware should take at most **98%** of the board's memory. Marlin firmware, espeically 2.0 and above can definitely exceed this limit by default. [This thread](https://github.com/MarlinFirmware/Marlin/issues/5216 "Reduding firmware size 1") and [this article](https://crosslink.io/2020/08/14/shrinking-marlin-2-0-how-to-reduce-firmware-size-for-8-bit-boards-and-still-use-a-bltouch-and-the-filament-sensor/ "Reducing firmware size 2") provide some insight into how to fix it. It comes down to removing unnecessary firmware features. I removed audio support and chose a smaller font size and logo which brought the compiled firmware under the memory limit with plenty of room at right around 124K (was at 30.1K before).

### 3. Flash using the Octoprint Firmware Updater Plugin

I haven't used this one yet, but likely will in the future since I regularly employ Octoprint to start/monitor 3D prints.

Assuming you are familiar with Octoprint and have the web interface open, go add the [Octoprint Firmware Updater](https://plugins.octoprint.org/plugins/firmwareupdater/ "octoprint plugin repository: firmware updater") plugin.

Check what board family your board is in, based upon the board's processor.

|	Board Family 	|	Flashing Tool	|
| ----------------- | ----------------- |
|	ATmega 			|	avrdude			|
|	AT90USB 		|	dfu-programmer	|
|	LPC1768 		|	lpc1768			|
|	SAM 			|	bossac			|
|	STM32 			|	stm32flash		|

> The CR-10 Melzi using the ATmega1284p is in the ATmega board family

To be flash an ATmega board, `avrdude` has to be installed on the Raspberry Pi, I used SSH to connect to the Pi:
`sudo apt-get update`
`sudo apt-get install avrdude`

Use this plugin configuration for the CR-10 ATmega board
 - AVR MCU type: ATmega1284p
 - path to avrdude: /usr/bin/avrdude  (can be found by running `which avrdude` when using SSH)
 - AVR Programmer Type:	Arduino
 - (Optional) Baud rate: Default or 115200 for cr-10

 The rest of the settings are optional so hit the *save* button and move on.

After the plugin is set up: 
1. Select the COM port
2. Select a firmware file
3. Click the right *flash from* button