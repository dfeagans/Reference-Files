# General Reflash Instructions for Kano Pixel ESP32 Module #

## ReFlash with MicroPython and [Pixel32](https://github.com/murilopolese/kano-pixel-kit-pixel32)
- Download the Reflash Tool from here: https://github.com/murilopolese/kano-pixel-kit-flash-tool
- Connect Kano Pixel unit to computer via USB. I didn't have to but if there are driver issues, quickest way to get all of them is installing the [Kano Pixel app](https://kano.me/landing/app/us). Otherwise, you have to manually install the drivers: [here](https://www.ftdichip.com/Drivers/D2XX.htm) and [here](https://www.ftdichip.com/Drivers/VCP.htm)
- Select appropriate port and do "Flash MicroPython + Pixel33."

## [Connecting Kano Pixel to Wifi](https://murilopolese.github.io/kano-pixel-kit-pixel32-docs/pixel32) ##
- The boot screens mean:
  - Orange: Trying to connect.
  - Blue: Created its own wifi network. It should be named something like PIXEL_KIT_XXXX but with a number instead of the XXXX.
  - Green: Connected to a wifi network.
  - Red: Tried to connect to a wifi network and failed.
- The boot screens display the Pixel's IP address using binary. It's read from left-to-right and down, with Red dot mean 1, no dot mean 0. It can be useful to use a [Binary-to-IP tool](https://www.browserling.com/tools/bin-to-ip).
- It will start by making it's own Wifi Network to start, as indicated by the Blue SCren. It will be then called "PIXEL_KIT_XXXX.".
- Connect to the Pixel's self-made Wifi Network and then navigate to http://192.168.4.1/ in your browser to start enterting commands.
- You can then connect the Pixel to your normal Wifi network by running: `saveWifiConf(ssid, passord)`, then `reset()`. I could NOT get it connected to the 5GHz, but connecting it to my 2.4GHz channel still put it on the same network obviously.
- If at any time you want to return to the state where it makes it's own network (in case you aren't around a WIFI network), you can return it to that state (Blue Screen) by holding down the two red buttons and turning it off/on.

## Putting it to Work ##
- https://murilopolese.github.io/kano-pixel-kit-pixel32-docs/ - General Documentation
- https://murilopolese.github.io/kano-pixel-kit-pixel32-docs/pixel-kit - Library for Rendering Screen Graphics and Reading Buttons/Knobs
- https://murilopolese.github.io/kano-pixel-kit-pixel32-docs/pixel-turtle - Toyish library for creating graphics. Not primary method.

## Alternative - OEM SDK ##
- https://kanocomputing.github.io/community-sdk-wiki/Home.html - Haven't touched but looks more promising, doesn't require reflash.
- https://github.com/KanoComputing/community-sdk-wiki - Actual repo for SDK.

## (UNREAD) Arduino Integration ##
- Full Feature List of ESP32 and Arduino Details: https://learn.sparkfun.com/tutorials/esp32-thing-hookup-guide
- Someone used the Kano Pixel with the Arduino IDE and resolved:
  - A0 is the knob - you get values from 4095 to 0.
  - The buttons look like they are active low...(I just used the Arduino example, to test this)
  - PIN 18 is the "B" button - red button on the right edge
  - PIN 23 is the "A" button - inner red button
  - PIN 25 is joystick right
  - PIN 26 is joystick left
  - PIN 27 is joystick button
  - PIN 34 is joystick down
  - PIN 35 is joystick up
