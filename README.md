
# Midea HVAC WiFi Dongle - v2 for EU-SK105

This repo is based on [Rene Klootwijk's repository](https://github.com/reneklootwijk/mideahvac-dongle). So majority of credits for all this go to him! Original ReadMe from him is also below.

I've made a couple of changes so the board, mainly to fit its size to the Midea EU-SK105 dongle that is common nowadays and to use a bit more common parts. List of changes below:
* Converted the KiCad project to KiCad 9
* Size of the PCB **without a case** is now exactly the size of the EU-SK105 **with a case**, which is 41x24mm. That means you can use it as 1:1 drop-in replacement.
* All pads are now "hand-solder" pads to allow for easier hand soldering, if you wish to
* The footprints for the LDO caps are now 0805 hand solder pads, compared to the larger pads they were before. This allows you to use usual ceramic caps, if you wish to (which are totally fine to use here), instead of the recommended tantalum caps. You can still use tantalum caps, if you wish so, as they also come in 0805 package size.
* Replaced a few resistors with a single resistor array. While this is not a common part people have at home, it saves space and is just less parts on the board
* Added iBOM file which is a great soldering help. Give it a look!
* Gerbers, BOM and CPL files have also of course been updated

Additional hint which also applies to Rene's original version: You can also use and ESP-07 instead which offers and external antenna, just in case you need better Wi-Fi reception ;)

That's what the board now looks like:
![How it looks](https://github.com/ezcGman/mideahvac-dongle/blob/master/images/mideahvac-dongle-v2.jpg?raw=true)

## PCB and Parts
* Gerbers, BOM and placement files can be found in the "production-files" folder
* The buttons are 3x4mm in size. Height is up to your liking. Can easily be found on AliExpress:
![Buttons](https://github.com/ezcGman/mideahvac-dongle/blob/master/images/buttons.png?raw=true)
* Specs of the USB jack, can also be found on AliExpress:
![USB Jack](https://github.com/ezcGman/mideahvac-dongle/blob/master/images/usb-jack.png?raw=true)
* LDO Caps (C1 & C2) can be either tantalum or ceramic: Officially should be tantalum, but ceramic work just fine. 
* And again: Remember that the ESP-12F is (in nearly all cases) interchangeable with an ESP-07 which offers an external antenna:
![Boards with ESP-07 and ESP-12F](https://github.com/ezcGman/mideahvac-dongle/blob/master/images/esp-07-12.jpg?raw=true)

I **highly** recommend taking a look at the iBOM file that can also be found in the "production-files" folder. It makes sourcing the parts and soldering so much easier!

## Flashing
Flashing the ESP can be done using any USB-to-UART adapter. I'm using an FT232RL FTDI that you can easily find on AliExpress. To wire it up, I recommend getting an USB-A-socket cable with open ends, so you can crimp a dupont at the open end. Wiring then looks like this:
![Wiring for flashing](https://github.com/ezcGman/mideahvac-dongle/blob/master/images/wiring-flashing.jpg?raw=true)
So TX from the dongle goes to RX on the UART adapter and RX from the dongle goes to TX on the UART adapter. Set the UART adapter to 5V. Yes, it is safe. You will **not** burn the ESP in this case! The board has two mosfets that will switch the pins to 3v3.
Now how to actually flash the board:
 1. Create a new device under "ESPHome Builder" in Home Assistant. Choose `ESP8266` as board
 2. You can find an example config under "esphome". Edit the config in ESPHome builder and substitute the placeholders and potentially the board, if you're using an ESP-07 instead of an ESP-12F.
 3. Install and download.
 4. Now open [ESPHome Web](https://web.esphome.io)
 5. Connect the UART adapter to your PC, but not the dongle just yet!
 6. Hold the "PRG" button on the dongle and keep holding it!
 7. Now connect the dongle to the UART adapter. Keep holding "PRG"!
 8. Click "Connect" on ESPHome Web, select the UART adapter, click "Install", select the bin you've just built and downloaded and flash it.
 9. As soon as it says "Erasing" or "Flashing", you can release the "PRG" button.
 10. Wait  for it to be installed and it will show up in your Wi-Fi and under "Devices" in Home Assistant

*(Original ReadMe below)*

---

This repository contains the hardware design of a WiFi dongle for Midea-like airconditioners and humidifiers.
Midea provides a WiFi dongle (SK102/SK103), either as optional feature or built-in, to control the appliances via a mobile application. This dongle uses and is dependent on the Midea cloud which does not expose a publically available API. The protocol used by the dongle has been reverse engineered, but newer version still require the cloud to obtain an encryption key or require an addition server simulating the Midea cloud. This means integration in an existing home automation system is not straight forward.

Besides the protocol the dongle uses to commumicate with the mobile app and the cloud, also the protocol that is used between the dongle and the appliances has been reverse engineered (at least the most important parts). This means the original dongle can be replaced by a custom dongle making it independent on any cloud.

Besides Midea itself other vendors of these type of appliances are also using the Midea cloud and dongle to support control via a mobile app. Examples of these vendors are:

* Qlima
* Artel
* Carrier
* Toshiba
* Idema

This design of a custom dongle can be used to replace the orignal dongle with a type A USB connector. The version with with a JST-XH connector needs a slight modification.

## Technical design

The design of the dongle is based on an ESP-12 which provides the WiFi interface and microcontroller to communicate with the appliance. Because the interface of the appliance is a 5V TTL serial port (UART) and the ESP-12 uses 3.3V and is not 5V tolerant, we need level shifters for the communication and a power regulator in order to power the dongle from the appliance.

The design has been made in KiCad and all files can be found in the KiCad directory.

<p align="center">
  <img src="./images/pcb.jpeg" /><br/>
  <em>The KiCad PCB design</em>
</p>

Note: The resulting PCB including the ESP-12 turned out to be 1 mm to long to fit into the location of the original dongle (in my Artel airconditioner). It was easy to locate it somewhere else, but pay attention whether the PCB would fit in your appliance. The dimensions of the dongle are: 55mmx20mm (l*w) without casing.

## Ordering and manufacturing

The PCB can be ordered by the manufacturer of your choosing. I had it manufactured and partly assembled by [JLCPCB](https://jlcpcb.com/). The files required to order the PCB including the SMT assembly of all components except the ESP-12 and USB connector can be found in the JLCPCB directory.

<p align="center">
  <img src="./images/top_assembled.jpeg" /><br/>
  <em>The assembled PCB</em>
</p>

The JLCPCB directory contains the following files:

* Gerber-Midea WiFi Dongle v1.zip, this zip contains all gerber files and the drill file.
* BOM-Midea WiFi Dongle.csv, this is the Bill Of Material file.
* CPL-Midea WiFi Dongle.csv, this is the Pick and Place file

Note: The ESP-12 and USB connector are not part of the BOM and need to be ordered somewhere else, JLCPCB could not deliver them. This also means you have to solder them yourself.

## Case

I also designed a case that can printed on a 3d printer. The .STL file is part of this repository.

<p align="center">
  <img src="./images/case.jpeg" /><br/>
  <em>The 3D printed case</em>
</p>

## Firmware

At this moment there are several options:
* Sören Beye created firmware to integrate dehumidifiers with Home Assistant: https://github.com/Hypfer/esp8266-midea-dehumidifier.
* Sergey V. Dudanov created an extension for ESPHome integrating aircondititioners with Home Assistant: https://github.com/dudanov/esphome.
* I created a node.js module to integrate airconditioners with anything you like, but you need to program: https://github.com/reneklootwijk/node-mideahvac. The firmware required on the dongle is esplink, a serial bridge: https://github.com/jeelabs/esp-link.
* To reverse engineer the protocol the original dongle is using to communicate with your appliance, you can use this custom dongle to capture this communication. See https://github.com/reneklootwijk/midea-uartsniffer.
