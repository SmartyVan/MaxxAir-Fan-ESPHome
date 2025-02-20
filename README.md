# MaxxAir Fan for Home Assistant via ESPHome using IR
Control your IR controlled MaxxAir from Home Assistant via ESPHome

This project is made up of two parts:
1. An **ESPHome YAML configuration** for an ESP microcontroller that utilizes an IR LED to control an Airxcel MaxxAir Fan with IR (7000k/7500k models).
2. A **Home Assistant Automation Blueprint** to set your Auto Fan preferences

## ESPHome Configuration
The included ESPHome configuration replicates a majority of the features available on the MaxxAir Fan IR remote:
- 10 speeds of an exhuasting fan (lid open)
- 10 speeds of an inward-pulling fan (lid open)
- 10 speeds of a downwards fan (lid closed) — a.k.a "Celing Fan Mode"
- Lid Open (fan off)
- Lid Close (fan off)

**In Home Assistant, the fan direction FORWARD will equate to AIR OUT on the MaxxAir Fan and REVERSE will equate to AIR IN on the MaxxAir Fan.**

This configuration creates 3 entities in Home Assistant:
1. A 10-speed fan with forward and reverse direction
2. A Ceiling Fan switch——turn Ceiling Fan Mode on and off
3. An Auto Fan switch——used by the [Auto Fan Automation Blueprint](#auto-fan)


The ESPHome YAML logic manages the states of the three entities based on the other states:
- Ceiling Fan Mode sets the Fan's direction to Reverse (as the MaxxAir Fan's Ceiling Fan Mode only pushes air downwards when the lid is closed)
- Manually changing the speed of the Fan will disable Auto Fan
- The Lid cover entity is updated to reflect the state of the lid. When the fan is off, the UP and DOWN (OPEN / CLOSE) buttons will open and close the lid of the MaxxAir fan without turning on the fan (passive ventiliation)
- Closing the lid when the fan is on will engage Ceiling Fan Mode
- Turning off Auto Fan will turn off the fan

## Auto Fan
The Auto Fan Automation Blueprint imitates the MaxxAir IR remote thermostatic control. When Auto Fan is enabled, your fan's speed will be adjusted based on your temperature thresholds, min/max fan speed, and a temperature sensor.

*The Auto Fan Automation Blueprint can be used seperately from the MaxxAir Fan ESPHome configuration and instead by used with any fan combined with any `input_boolean` or `switch` and a temperature sensor.*

