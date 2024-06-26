---
title: LSC Light White Ambiance 3000272
date-published: 2023-04-17
type: light
standard: eu
---

## Warning

The latest LSC Light White Ambiance 3000272 devices use the Tuya BK7231T module, which is not compatible with ESPHome!

## Notice

- This light is flashable using the latest tuya-cloudcutter with a compiled ESPHome binary.
- The LEDs are controlled by 2 PWM output (Cold and White channels).
- The PWM are ingested into a BP5772 chip (Tuya).

## Product Images

![light with box](image.png)

## GPIO Pinout

| Pin    | Function                            |
| ------ | ----------------------------------- |
| P24    | Cold White (PWM)                    |
| P26    | Warm White (PWM)                    |

## Basic configuration

```yml
substitutions:
  name: lsc-light-warm-01
  friendly_name: lsc-light-warm-01
esphome:
  name: ${name}
  friendly_name: ${friendly_name}

libretuya:
  board: generic-bk7231t-qfn32-tuya
  framework:
    version: dev
  
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

# Enable the Web Server component 
webserver:

output:
  - platform: ledc
    id: output_cold_white
    pin: P24
  - platform: ledc
    id: output_warm_white
    pin: P26


# Example configuration entry
light:
  - platform: cwww
    name: ${friendly_name}
    id: the_light
    cold_white: output_cold_white
    warm_white: output_warm_white
    cold_white_color_temperature: 6536 K
    warm_white_color_temperature: 2000 K
    constant_brightness: true
    effects:
      - random:
          name: "Slow Random"
          transition_length: 30s
          update_interval: 30s
      - random:
          name: "Fast Random"
          transition_length: 4s
          update_interval: 5s
      - strobe:
      - flicker:
          alpha: 50% #The percentage that the last color value should affect the light. More or less the “forget-factor” of an exponential moving average. Defaults to 95%.
          intensity: 50% #The intensity of the flickering, basically the maximum amplitude of the random offsets. Defaults to 1.5%.
      - lambda:
          name: Throb
          update_interval: 1s
          lambda: |-
            static int state = 0;
            auto call = id(the_light).turn_on();
            // Transtion of 1000ms = 1s
            call.set_transition_length(1000);
            if (state == 0) {
              call.set_brightness(1.0);
            } else {
              call.set_brightness(0.01);
            }
            call.perform();
            state += 1;
            if (state == 2)
              state = 0;

## Blink the light if we aren't connected to WiFi.
## Uncomment below
#interval:
#- interval: 500ms
#  then:
#  - if:
#      condition:
#        not:
#          wifi.connected:
#      then:
#        - light.turn_on:
#            id: the_light
#            brightness: 50%
#            transition_length: 0s
#        - delay: 250ms
#        - light.turn_off:
#            id: the_light
#            transition_length: 250ms
```
