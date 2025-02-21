# IR MaxxAir Fan Control in Home Assistant via ESPHome
Control your [IR (infrared) enabled MaxxAir Fan](https://amzn.to/4gPCY9p) from Home Assistant via ESPHome.

This ESPHome configuration and Automation Blueprint are free and open source. Consider joining a [Smarty Van YouTube Membership tier](https://www.youtube.com/channel/UCtDZoJa423TcB1ri-ZjDoqA/join) to show some love. 🥰🤓

## Watch the YouTube video about this project
[<img width="540" alt="Smarty Van YouTube video" src="https://img.youtube.com/vi/ckXplzLgLZw/maxresdefault.jpg"/>](https://www.youtube.com/watch?v=ckXplzLgLZw)

## Project Components
This project is made up of two parts:
1. **[ESPHome](https://esphome.io) YAML configuration:** for an ESP microcontroller that utilizes an IR LED to control an Airxcel MaxxAir Fan with IR ([7000k/7500k models](https://amzn.to/4gPCY9p)).
2. **[Home Assistant](https://www.home-assistant.io) Automation Blueprint:** to enable Auto Fan and set your preferences

### Home Assistant Entities Created
<img width="935" alt="Home Assistant Entities Added" src="https://github.com/user-attachments/assets/710301ec-5e1e-481d-b165-c50de1f5228e" />

### Automation Blueprint Options
<img width="1254" alt="Smarty Van Auto Fan Automation Blueprint" src="https://github.com/user-attachments/assets/4a3024f7-c142-4011-bd94-b67a8f3651b5" />

## ESPHome Configuration
The included ESPHome configuration replicates a majority of the features available on the MaxxAir Fan IR remote:
- 10 speeds of an AIR OUT fan (lid open)
- 10 speeds of an AIR IN fan (lid open)
- 10 speeds of AIR IN (lid closed) — a.k.a "Celing Fan Mode"
- Lid Open (fan off)
- Lid Close (fan off)

**In Home Assistant, the fan direction FORWARD will equate to AIR OUT on the MaxxAir Fan and REVERSE will equate to AIR IN on the MaxxAir Fan.**

This configuration creates 4 entities in Home Assistant:
1. **Fan:** A 10-speed fan with forward and reverse direction
2. **Switch:** A Ceiling Fan switch——turn Ceiling Fan Mode on and off
3. **Switch:** An Auto Fan switch——used by the [Auto Fan Automation Blueprint](#auto-fan)
4. **Cover:** A cover to represent and control the state of the fan's lid/cover


The ESPHome YAML logic manages the states of all four entities based on user input to the other entities much like the MaxxAir Fan remote:
- Enabling Ceiling Fan Mode automatically sets the Fan's direction to Reverse (as the MaxxAir Fan's Ceiling Fan Mode only pushes air downwards when the lid is closed)
- Manually controlling the Fan will disable Auto Fan
- The state of the Lid cover entity is updated to reflect the state of the lid
- When the Fan is off, the Lid cover entity's UP and DOWN (OPEN / CLOSE) buttons will open and close the lid of the MaxxAir Fan without turning on the fan (passive ventiliation)
- When the fan is on, closing the lid will engage Ceiling Fan Mode
- When the fan is on, disabling Ceiling Fan Mode will open the lid and the fan will continue operating in REVERSE (AIR IN) 
- Turning off Auto Fan will turn off the fan

### Install ESPHome Configuration
- Create a new ESPHome deivce called "MaxxAir Fan Control" in the [ESPHome add-on](https://esphome.io/guides/getting_started_hassio.html)
- Choose ESP8266 (or any board, we'll overwrite this in the following steps)
- After the device has been created, skip the initial install
- Edit the configuration of your new device and copy the configuration yaml in this repo into your ESPHome device configuration, overwriting the existing configuration
- Change your board and pin definitions if necessary then save and close
- Open Secrets (top right) in the ESPHome builder and add [secrets](https://esphome.io/components/wifi.html#wifi-component) for:
  - `wifi_ssid`
  - `wifi_password`
- Add [OTA password](https://esphome.io/components/ota/esphome.html) and [API encryption](https://esphome.io/components/api.html#native-api-component) to your configuration if desired
- Save and Install (wirelesly if you can, or use a USB and [ESPHome Webtools](https://web.esphome.io))

## Auto Fan
The Auto Fan Automation Blueprint imitates (and improves) the MaxxAir IR remote's thermostatic control. When Auto Fan is enabled, your fan's speed will be adjusted based on configurable temperature thresholds, minimum and maximum fan speeds, and a temperature sensor of your choosing.

Auto Fan never turns your fan off, only assigns speeds based on temperature. Turn off the Fan or Auto Fan to power off the Fan.

*The Auto Fan Automation Blueprint can be used with any Home Assistant fan, though it will only send 10 speeds (10%, 20%, 30%, etc)*

### Install Auto Fan Blueprint

To install the Smarty Van Auto Fan Blueprint, navigate to your Home Assistant [Blueprints](https://my.home-assistant.io/redirect/blueprints/) and click the "Import Blueprint" button.
Paste the [link to the bluprint](https://github.com/SmartyVan/MaxxAir-Fan-ESPHome/blob/main/smarty_van_autofan_blueprint.yaml) in this repo.

# Hardware

Building your ESP IR device requires four items:
- ESP8266 (or ESP32)——This configuration assumes a [Wemos D1 Mini](https://amzn.to/4177tBZ) but any ESP8266/32 should work (as long as you update pin asignments accordingly)
- An [IR LED](https://amzn.to/3Dd07oq)
- A [220Ω resistor](https://amzn.to/4gQCPmh)
- A 5v power supply for your ESP——I've used a [12v to 5v down converter](https://amzn.to/4gLhs5T)

## Wiring Diagram

- Annode of LED through resistor to 3V3
- Cathode of LED to GPIO pin
<img width="745" alt="Screenshot 2025-02-20 at 5 16 07 PM" src="https://github.com/user-attachments/assets/2293c199-a8dc-461a-a5ec-8d958d2c5503" />
