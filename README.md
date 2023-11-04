# PianoLED-ESP32 V1

![image](https://github.com/serifpersia/pianoled-esp32/assets/62844718/b79ec631-f5a9-41ff-8bf0-f93228759d48)

**PianoLED-ESP32** is a simple web-based interface that allows you to control a WS2812 5V LED Strip with a USB MIDI Piano. You can configure all effects, colors, and parameters through a locally hosted webserver on the ESP32 board.

## Supported Boards

- ESP32-S2
- ESP32-S3

## Supported LED Strips

- WS2812 5V 144/m
- WS2812 5V 72/m (1:1 ratio)
- *88/76 Keys need 176 leds of 144leds/m density so more than 1m of strip is needed, get 2m one and cut the excess(after 176th led).

## Installation

The installation is relatively simple. If you need help or want to join my PianoLED Community Discord Server, feel free to hop in:

<a href="https://discord.gg/QAh7wJHrRM"><img src="https://discordapp.com/api/guilds/1077195120950120458/widget.png?style=banner2" width="25%"></a>

This project also supports MIDI over local network so you can use midi device on your pc with this project there is no latency. For Windows use rtpMIDI software,use esp's ip and port 5004. ESP must be in non AP mode to be able to use MIDI over network

### Hardware

To get started with PianoLED on the ESP platform, you have two options to choose from:

#### Option 1: ESP32-S2 Single or Dual USB Port Dev Board

- Create a DIY USB OTG/Female USB cable:
  - Cut down a short(under 15cm) USB cable with a female port on one end.
  - Splice 4 female-end jumper wires with the 4 USB wires, matching USB wire colors with jumper cable colors.
- Connect this DIY cable to pins on the ESP32-S2 as follows:
  - USB - ESP32-S2 Board Pins:
    - Red Wire - 5v Pin
    - Black Wire - GND Pin
    - White Wire - 19 Pin
    - Green Wire - 20 Pin
  - LED STRIP - ESP32-S2 Board Pins:
    - Red Wire - 5v Pin
    - White Wire - GND Pin
    - Green Wire - Data Pin (default 18)

If you use usb otg port on s2/s3 you might have esp power issue, to fix this bypass the usb otg port and do the 
diy usb micro b/c to Female A port short cable and connect wires directly to esp32's 5V,GND, 19 &  20 pins.

![image](https://github.com/serifpersia/pianoled-esp32/assets/62844718/cea8ebeb-09c5-46e9-a028-67c5447ad0f3)


![image](https://github.com/serifpersia/pianoled-esp32/assets/62844718/9ea3a1e8-52e6-40e1-9069-58c918e9e6ef)


For power, you can use the USB port of the board. The default current limit is set to 450mA. If the plan is to power the LED STRIP separately from the same or different 5V DC supply, make sure both power source grounds are connected to the ground of the ESP32 Board.

#### Option 2: ESP32-S3 Dual USB Port Dev Board

This is more of a plug & play setup. Depending on whether your board has pre-soldered pin headers and USB-OTG pads soldered, you have to bridge the USB-OTG pads,for boards without this feature, for them to work with your USB MIDI Device you need to connect the 5V pin to 5v wire of your USB cable connected to the USB OTG/Host capable port. This applies for S2 as well,some S2 boards can also have 2 usb ports one for USB Host but you may need to use 5v Pin if MIDI device doesn't function.

The same LED scheme applies to ESP-32S3. A spare COM USB port can be used for basic power, but if you need brighter LEDs, consider powering the LED strip externally.

![image](https://github.com/serifpersia/pianoled-esp32/assets/62844718/a089640f-113e-47b1-8c88-8e38e4728295)


### Software

You'll need the following software:

- Arduino IDE 1.8.x (If you plan to upload the main PianoLED-ESP32 for the Barebones sketch, you can use Arduino IDE 2.x since the ESP32 Upload Sketch Data Plugin is not needed for the barebones sketch).

Install the following libraries:

- ESP32 Arduino Core and the latest esp32 board package (follow the [official guide](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html)).
- Libraries needed for barebones are: WiFiManager, ElegantOTA, and FastLED.
- ESPAsyncWebServer and AsyncTCP will need to be installed manually by downloading the zips and installing them via the Sketch Manage Libraries "install library from zip" in the Arduino IDE.

[ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)

[AsyncTCP](https://github.com/me-no-dev/AsyncTCP)

[Arduino core for ESP32](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html)

Download the barenes folder which will be used for initial setup, you could use the main folder but you need to install more libraries(latest main is always most up-to-date code wise and release bins might have older version of the project compiled). Barebones is static it will not change unless some other new idea gets more useful.

After selecting your S2/S3 dev board from the tools>boards menu in Arduino IDE, a few things to check before uploading. For S3, make sure your USB MODE is set to TinyUSB. In the tools menu, select the port of your board and click the > arrow to start uploading to the ESP32 Board. If you see "Leaving... Hard resetting via RTS pin...," your upload was successful.

On your WiFi-capable device (usually a smartphone), you will see "PianoLED AP WiFi." The password is "pianoled99." After connecting to it, in your browser, type 192.168.4.1 if you didn't already see the WiFi portal. Here you can go to "Configure" and connect to your local WiFi network. The ESP32 board will restart and be connected to this network.

Download the release binaries for your board. Connect to your ESP32 webserver via "pianoled.local" or by IP address (the LED strip will indicate the IP address), or simply get this info by connecting the ESP32 board back to the PC and, in Arduino Tools menu, find Serial Monitor, press the reset button on ESP32, and you will find your IP if you can't read the IP from the LED strip or "pianoled.local" is not working in your device's web browser.

![image](https://github.com/serifpersia/pianoled-esp32/assets/62844718/91beaa8e-c168-46cb-b048-daac8cc76df6)


The final step is to click the update button on the webserver and upload the bin files you downloaded from the release page. You can update the firmware or filesystem in any order you want, but make sure you have both uploaded. Since the ESP32 has the main PianoLED code uploaded now, connecting to the same IP or "pianoled.local" will bring you to the main web interface.

If you want ESP32 to act only as AP WiFi, you can connect pin 10 and GND pins together to be in AP Mode. By doing so, saved network settings get removed, so when you remove the pin 10-GND connection, the ESP32 will be in WifiManager AP mode PianoLED Setup, allowing you to go to the portal and connect to your desired network once more.
