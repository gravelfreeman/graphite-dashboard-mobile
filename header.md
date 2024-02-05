# Header

On all the pages I'm using [Mushroom Chips Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/chips.md) for showing various information. On the homepage I like to show the outdoor temperature, alarm system and opened lights. For the rooms I'm showing the room temperature, room humidity and opened lights.

## 1. Homepage header

### 1.1 Chip card template for the homepage

```
type: custom:mushroom-chips-card
chips:
  - type: weather
    entity: weather.forecast
    show_conditions: false
    show_temperature: true
    tap_action:
      action: navigate
      navigation_path: /lovelace/ROOMNAME_FOR_EXTERIOR
  - type: template
    entity: alarm_control_panel.alarmo
    icon_color: |-
      {{ 'green' if states(entity) == 'armed_away' else 
         'green' if states(entity) == 'armed_home' else
         'orange' if states(entity) == 'arming' else
         'red' if states(entity) == 'triggered' else
         'disabled' }}
    tap_action:
      action: more-info
    hold_action:
      action: navigate
      navigation_path: /lovelace/security
    double_tap_action:
      action: none
    icon: mdi:security
    content: |-
      {{ 'Armed' if states(entity) == 'armed_away' else 
         'Armed' if states(entity) == 'armed_home' else
         'Alarm' if states(entity) == 'triggered' else
         'Arming' if states(entity) == 'arming' else
         '' }}
    picture: ''
  - type: entity
    entity: sensor.TOTAL_LIGHTS_SENSOR
    icon_color: amber
    tap_action:
      action: navigate
      navigation_path: /lovelace/lights
    hold_action:
      action: call-service
      service: light.turn_off
      target:
        area_id:
          - ROOM_NAME # LIST ALL ROOM AREA_ID HERE
          - ROOM_NAME
          - ROOM_NAME
    double_tap_action:
      action: none
alignment: center
```

### 1.2 Helper Template to show total number of lights ON
`{{ states.light|selectattr("state", "equalto", "on")|list|length }}`
_add this as an Helper Template_

![image](https://github.com/gravelfreeman/ha-mobile-dashboard/assets/44218371/a92fad82-7ee5-4d9c-ba0d-4c4c9211804d)

## 2. Individual rooms header

### 2.1 Chip card template for individual room

```
type: custom:mushroom-chips-card
chips:
  - type: entity
    entity: sensor.THERMOSTART_ROOM_NAME
    use_entity_picture: true
    icon_color: orange
  - type: entity
    entity: sensor.HUMIDITY_ROOM_NAME
    use_entity_picture: true
    icon_color: cyan
  - type: conditional
    conditions:
      - condition: numeric_state
        entity: sensor.LIGHTS_ROOM_NAME
        above: 0
    chip:
      type: entity
      entity: sensor.LIGHTS_ROOM_NAME
      content_info: none
      icon_color: amber
      tap_action:
        action: none
      hold_action:
        action: call-service
        service: light.turn_off
        target:
          area_id: ROOM_NAME
        data: {}
      double_tap_action:
        action: none
alignment: center
```

### 2.2 Helper Template to show total number of lights ON per room
```
{%- set search_state = 'on' %}
{%- set search_area = 'ROOM_NAME' %}
{%- set ns = namespace(lights=[]) %}
{%- for light in states.light | selectattr('state','eq', search_state) if area_name(light.entity_id) == search_area %}
  {%- set ns.lights = ns.lights + [ light.entity_id ] %}
{%- endfor %}
{{ ns.lights | length }}
```
_add this as an Helper Template for each room_

![image](https://github.com/gravelfreeman/ha-mobile-dashboard/assets/44218371/a08e106b-d926-4035-b875-f3462882b881)
