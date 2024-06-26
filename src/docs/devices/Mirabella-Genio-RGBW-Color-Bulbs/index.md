---
title: Mirabella Genio RGBW Color Bulbs
date-published: 2023-04-12
type: light
standard: global
board: esp8266
---

The Mirabella Genio is a Tuya-based smart bulb sold by Kmart in
Australia.

![mirabella-genio](/mirabella-genio-b22-rgbw.jpg)

## Basic Configuration

``` yaml
esphome:
  name: example-device
  friendly_name: Example Device

esp8266:
  board: esp01_1m
  
# OTA flashing
ota:
  - platform: esphome

wifi: # Your Wifi network details
  
# Enable fallback hotspot in case wifi connection fails  
  ap:

# Enabling the logging component
logger:

# Enable Home Assistant API
api:

# Enable the captive portal
captive_portal:

output:
  - platform: esp8266_pwm
    id: output_red
    pin: GPIO4
  - platform: esp8266_pwm
    id: output_green
    pin: GPIO12
  - platform: esp8266_pwm
    id: output_blue
    pin: GPIO14
  - platform: esp8266_pwm
    id: output_white
    pin: GPIO5

light:
  - platform: rgbw
    name: "rgbw_e27_01"
    id: light
    red: output_red
    green: output_green
    blue: output_blue
    white: output_white

    # Ensure the light turns on by default if the physical switch is actuated.
    restore_mode: ALWAYS_ON
```
