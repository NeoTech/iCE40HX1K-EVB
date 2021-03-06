This firmware has to be uploaded to OLIMEXINO-32u4 in order to use it as an iCE40 programmer.


### Using Arduino IDE ###

This firmware can be uploaded via Arduino IDE. To compile it use SPIFlash 2.2.0 library for 
Winbond Flash Memory by Prajwal Bhattaram - Marzogh.

To load this library via Internet go to Sketch -> Include Library -> Manage Libraries. Refer
to the pictures in this folder for more information on the library installation procedure.


### Using PlatformIO ###

Install PlatformIO, use this guide for details http://docs.platformio.org/en/stable/installation.html

Build & Upload:

    platformio run
    platformio run --target upload

Alternatively you can use SPI programmer to burn firmware into OLIMEXINO-32u4, e.g.:

    avrdude -p atmega32u4 -P /dev/ttyUSB0 -c buspirate -b115200 -D -Uflash:w:[.pioenvs/leonardo/firmware.hex]:i
