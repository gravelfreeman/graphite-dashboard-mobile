
## 1. Header

I plan to add an header with important information about the security system. Again using Mushroom Chips.

## 2. Security System

This security system is based on [Alarmo](https://github.com/nielsfaber/alarmo). You'll first have to setup Alarmo before setting up this card. You can create this card by installing [Mushroom Alarm Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/alarm-control-panel.md). As you can see I haven't added the bulky digit panel. Instead I opted for it to show only when arming or disarming the alarm system which make this page more clean.

```
type: custom:mushroom-alarm-control-panel-card
entity: alarm_control_panel.alarmo
states:
  - armed_home
  - armed_away
primary_info: state
secondary_info: last-changed
layout: horizontal
```

## 3. Security System

This will most likely depend on what your security camera setup is. Although I highly recommend Frigate NVR as it's well integrated in Home-Assistant and is FOSS. Since it's a mobile dashboard I've setup all my cameras in the widget which will show the latest camera that had detected movement. You can quickly scroll through your cameras by using your thumb and swiping left or right. Again just like Alarmo, I suggest that you look up all the options and set it up as you like.

```
type: custom:frigate-card
cameras:
  - camera_entity: camera.NAME
    live_provider: auto
    icon: mdi:cctv
    title: CAMERA_NAME
  - camera_entity: camera.NAME
    icon: mdi:cctv
    title: CAMERA_NAME
  - camera_entity: camera.NAME
    hide: false
    icon: mdi:cctv
    title: CAMERA_NAME
view:
  dark_mode: 'off'
  camera_select: live
  update_cycle_camera: true
  scan:
    enabled: true
  default: live
dimensions:
  aspect_ratio_mode: dynamic
performance:
  style:
    border_radius: true
    box_shadow: false
live:
  preload: false
  draggable: true
  zoomable: true
  controls:
    builtin: true
    next_previous:
      style: none
    title:
      mode: none
    timeline:
      mode: none
      style: stack
      media: all
  show_image_during_load: true
  auto_pause: hidden
  auto_mute: unselected
  auto_unmute: selected
  transition_effect: none
  auto_play: visible
  lazy_unload: hidden
  lazy_load: true
menu:
  buttons:
    frigate:
      enabled: false
    camera_ui:
      enabled: true
    timeline:
      alignment: matching
    media_player:
      alignment: opposing
    snapshots:
      enabled: false
    fullscreen:
      alignment: opposing
    mute:
      enabled: true
      alignment: opposing
      priority: 50
  position: bottom
  style: hover-card
  alignment: left
```

## 4. Sensors

Here I'm listing all the sensors. I myself only have door sensors but you might want to move around this section if you have other kind of sensors like window sensors. In my case I'm using the first column for door sensors and the second column for motion sensors.

I've achieved my section title using Subtitle line in a [Mushroom Title Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/title.md).

```
type: custom:mushroom-title-card
title: ''
subtitle: Détecteurs de mouvement
```

Followed by the sensors themselves in an horizontal stack card containing 2 vertical stacks, one for each columns. Everything is colored depending on the state. I even added a red badge template for when the battery power is under 10%. Make sure to update these templates with according to your sensor ids.

```
type: horizontal-stack
cards:
  - type: vertical-stack # FIRST COLUMN FOR DOOR SENSORS
    cards:
      - type: custom:mushroom-template-card
        primary: DOOR_NAME
        secondary: |-
          {{ 'Closed' if states(entity) == 'off' else
             'Open' if states(entity) == 'on' else
             'Inactive' }}
        icon: |-
          {{ 'mdi:door-closed' if states(entity) == 'off' else
             'mdi:door-open' if states(entity) == 'on' else
             'mdi:door' }}
        icon_color: |-
          {{ 'grey' if states(entity) == 'off' else
             'yellow' if states(entity) == 'on' else
             'disabled' }}
        multiline_secondary: false
        fill_container: false
        entity: binary_sensor.DOOR_NAME
        badge_icon: >-
          {{ 'mdi:battery' if states('sensor.DOOR_NAME_BATTERY') | int <
          10 }}
        badge_color: red
        tap_action:
          action: none
        hold_action:
          action: none
        double_tap_action:
          action: none
  - type: vertical-stack # FIRST COLUMN FOR DOOR SENSORS
    cards:
      - type: custom:mushroom-template-card
        primary: DOOR_NAME
        secondary: |-
          {{ 'Closed' if states(entity) == 'off' else
             'Open' if states(entity) == 'on' else
             'Inactive' }}
        icon: |-
          {{ 'mdi:door-closed' if states(entity) == 'off' else
             'mdi:door-open' if states(entity) == 'on' else
             'mdi:door' }}
        icon_color: |-
          {{ 'grey' if states(entity) == 'off' else
             'yellow' if states(entity) == 'on' else
             'disabled' }}
        multiline_secondary: false
        fill_container: false
        entity: binary_sensor.DOOR_NAME
        badge_icon: >-
          {{ 'mdi:battery' if states('sensor.DOOR_NAME_BATTERY') | int <
          10 }}
        badge_color: red
        tap_action:
          action: none
        hold_action:
          action: none
        double_tap_action:
          action: none
  - type: vertical-stack # SECOND COLUMN FOR MOTION SENSORS
    cards:
      - type: custom:mushroom-template-card
        primary: ROOM_NAME
        secondary: |-
          {{ 'Active' if states(entity) == 'off' else
             'Detected' if states(entity) == 'on' else
             'Inactive' }}
        icon: |-
          {{ 'mdi:leak' if states(entity) == 'off' else
             'mdi:leak-off' if states(entity) == 'on' else
             'mdi:door' }}
        icon_color: |-
          {{ 'grey' if states(entity) == 'off' else
             'cyan' if states(entity) == 'on' else
             'disabled' }}
        multiline_secondary: false
        fill_container: false
        entity: binary_sensor.MOTION_SENSOR_NAME
        badge_icon: >-
          {{ 'mdi:battery' if states('sensor.MOTION_SENSOR_NAME_BATTERY') |
          int < 10 }}
        badge_color: red
        tap_action:
          action: none
        hold_action:
          action: none
        double_tap_action:
          action: none
  - type: vertical-stack # SECOND COLUMN FOR MOTION SENSORS
    cards:
      - type: custom:mushroom-template-card
        primary: ROOM_NAME
        secondary: |-
          {{ 'Active' if states(entity) == 'off' else
             'Detected' if states(entity) == 'on' else
             'Inactive' }}
        icon: |-
          {{ 'mdi:leak' if states(entity) == 'off' else
             'mdi:leak-off' if states(entity) == 'on' else
             'mdi:door' }}
        icon_color: |-
          {{ 'grey' if states(entity) == 'off' else
             'cyan' if states(entity) == 'on' else
             'disabled' }}
        multiline_secondary: false
        fill_container: false
        entity: binary_sensor.MOTION_SENSOR_NAME
        badge_icon: >-
          {{ 'mdi:battery' if states('sensor.MOTION_SENSOR_NAME_BATTERY') |
          int < 10 }}
        badge_color: red
        tap_action:
          action: none
        hold_action:
          action: none
        double_tap_action:
          action: none
```
