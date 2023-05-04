ESPHome Firmware for Sonoff T Series wallswitches

Multifunction Version
This firmware lets you configure on-the-fly how the device will work if you single click any of it's touch surfaces
- Switch toggle with relay activation (factory default)
- Switch toggle without relay activation (double-click to toggle configuration)
- Long-press to choose) Event button as a single click (read notes below)

Multitouch Version
Now you can use single or double click and long-press to expand the functions of this wallswitch.
For example, you can single touch to turn on a smart lamp without activation the relay, you can double-click to activate the relay to turn on a dump lamp, you can long-press to turn off both lamps (use your imagination)
Everything is configurable from the Home Assistant UI and everything is -of-course- local.

Notes:
If a touch surface is configured as a momentary button/switch (we call it "event button") then an automation must be implemented to listen to that event.
For example below we use a helper (input_boolean.test_toggle). Be sure to replace "$friendly_name" with your device name.
This automation script could be expanded to include all event buttons using a choose function
The reason that no automation in included in the firmware is that most of the times it's a config-and-forget situation.
Options are stored on the ESP chip every 10 minutes.
To avoid conffusion, I suggest not to modify entities, only name and the icons.

alias: event buttons toggle
description: example
mode: single
trigger:
  - platform: event
    event_type: esphome.button_pressed
    event_data:
      message: $friendly_name button1.1 is pressed
  - platform: event
    event_type: esphome.button_pressed
    event_data:
      message: $friendly_name button1.2 is pressed... ...etc
condition: []
action:
  - service: input_boolean.toggle
    data: {}
    target:
      entity_id: input_boolean.test_toggle
  - delay: 1s # if events are too quick
