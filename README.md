# ha-graphite-mobile-dashboard
Sharing my custom dashboard since my friend asked for it. Since he offered me to help with the code I thought it would make sense to host it on my Github. Might as well share it publicly so that more people can help.

The goal of this mobile dashboard is to fix 2 things that I think are missing from Home Assistant. First looking good. Second being actually practical. Home-Assistant is a very good tool but I think it's ugly and all over the place. This dashboard isn't ready all of the box. You'll still have the pleasure to setup and learn Home-Assistant to your liking. It's more like a way to organize your dashbaord to achieve a clean look while keeping it practical.

## 1. Header

On all the pages I'm using [Mushroom Chips Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/chips.md) for showing various information. On the homepage I like to show the outdoor temperature, alarm system and opened lights. For the rooms I'm showing the room temperature, room humidity and opened lights.

### 1.1 Homepage header

#### 1.1.1 Chip card template for the homepage

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

#### 1.1.2 Helper Template to show total number of lights ON
`{{ states.light|selectattr("state", "equalto", "on")|list|length }}`
_add this as an Helper Template_

![image](https://github.com/gravelfreeman/ha-mobile-dashboard/assets/44218371/a92fad82-7ee5-4d9c-ba0d-4c4c9211804d)

### 1.2 Individual rooms header

#### 1.2.1 Chip card template for individual room

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

#### 1.2.2 Helper Template to show total number of lights ON per room
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

## 2. Main Page

The main page is dynamic which means that I'm only showing the widgets that are in an active state from the other rooms.

I like to show a greeting using the [Mushroom Title](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/title.md). It kinda help formatting the page and make it look like an official device. In the subtitle of that card I'm declaring what's up for dinner tonight. You can add this code in the subtitle to do the same if you have [Mealie HACS](https://github.com/mealie-recipes/mealie-hacs).

``Le souper de ce soir sera {{ states('sensor.mealie_todays_meal') }}.``

Right under I like to show who's home or away with [Mushroom Person Cards](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/person.md). You can achieve it with an horizontal stack and this code example

```
type: horizontal-stack
cards:
  - type: custom:mushroom-person-card
    entity: person.REPLACEME
    layout: horizontal
    icon_type: entity-picture
  - type: custom:mushroom-person-card
    entity: person.REPLACEME
    icon_type: entity-picture
```

## 2.1 Dynamic Cards

A few examples would be if there's music playing, if there's laundry ongoing, etc. You can quickly set this up by copy pasting cards from your room and integrate them in a [Conditional Card](https://www.home-assistant.io/dashboards/conditional/). Set the condition so that it only appears if the state is active.

Here's an example with Spotify that will only be showing when there's a song playing

```
type: conditional
conditions:
  - condition: state
    entity: media_player.spotify
    state: playing
card:
  type: media-control
  entity: media_player.spotify
  name: Spotify
  tap_action: null
  action: navigate
  navigation_path: /lovelace/media
```

## 3. Rooms

Here I'm using [UI-Lovelace-Minimalist room cards](https://ui-lovelace-minimalist.github.io/UI/usage/cards/card_room/) but without UI-Lovelace-Minimalist which I find it to be too much overhead and I like to keep my install as minimal as possible. I'm using an [Horizontal Stack Card](https://www.home-assistant.io/dashboards/horizontal-stack/) in order to show 2 room card per row which I found to be the best for a mobile dashboard. Those room cards are the only way to navigate the rooms in the dashboard which leave the dashboard menu to be used for other stuff. The room card will display:

- Room name and icon
- Up to 4 customizable icons (ex. climate, lights, appliance)
- Room temperature and humidity
- Customizable color

I like to use the first icon for the climate control and the second icon for lights control. Basically those icons are a shortcut to avoid navigating in the room and control that room faster. I reserve the third and fourth icons in case that a room has an appliance I'd like to quickly access. Examples would be media center in living room, oven in kitchen, laundry appliances in bathroom, etc.

### 3.1 Add the card-room template

In order to be able to use the room cards in your dashboard you'll have to edit your dashboard in the Raw Configuration Editor. Copy the code from [room-card.yaml](https://github.com/gravelfreeman/ha-mobile-dashboard/blob/main/rooms.yaml
) into your dashboard raw yaml configuration file.

You must paste it before the data of your configuration file. It should be before your first page and exactly before:

`title: YOUR_TITLE`

### 3.2 Add a custom card-room in your dashboard

The way it work is that you creating a custom template card for 2 rooms that'll be diplayed horizontally. Add as many as you want and they'll nicely work together. Here's an example for 2 rooms.

```
type: vertical-stack
title: Pièces
cards:
  - type: horizontal-stack
    cards:
      - type: custom:button-card
        template:
          - card_room
          - purple_no_state # THIS VALUE CHANGE THE COLOR OF YOUR CARD
        name: ROOM_NAME
        icon: mdi:ROOM_ICON
        entity: sensor.THERMOSTAT_ROOM_NAME
        tap_action:
          action: navigate
          navigation_path: /lovelace/ROOM_PATH
        variables:
          label_use_temperature: true
          label_use_brightness: false
          humidity: sensor.HUMIDITY_ROOM_NAME
          entity_1: # THIS EXAMPLE IS FOR CONTROLLING CLIMATE IN THAT ROOM
            entity_id: climate.THERMOSTAT_ROOM_NAME
            templates:
              - climate # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: more-info
            hold_action:
              action: none
            double_tap_action:
              action: none
            icon: mdi:ICON_NAME
          entity_2: # THIS EXAMPLE IS FOR TURNING OFF ALL LIGHTS IN THAT ROOM
            entity_id: sensor.LIGHTS_ROOM_NAME # SEE 1.2.2 IN THIS GUIDE TO CREATE THIS HELPER
            templates:
              - light
            tap_action:
              action: call-service
              service: light.toggle
              service_data:
                area_id:
                  - ROOM_NAME
            hold_action:
              action: none
            double_tap_action:
              action: none
          entity_3: # THIS EXAMPLE IS FOR A CONFIGURED APPLE TV IN HA
            entity_id: media_player.REPLACETHIS
            templates:
              - media_player # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: navigate
              navigation_path: /lovelace/ROOM_PATH
            hold_action:
              action: call-service
              service: remote.send_command
              service_data:
                device_id: REPLACETHIS
                command:
                  - suspend
                  - wakeup
              icon: mdi:television # EXAMPLE FOR MEDIA CENTER
            double_tap_action:
              action: none
      - type: custom:button-card
        template:
          - card_room
          - purple_no_state # THIS VALUE CHANGE THE COLOR OF YOUR CARD
        name: ROOM_NAME
        icon: mdi:ROOM_ICON
        entity: sensor.THERMOSTAT_ROOM_NAME
        tap_action:
          action: navigate
          navigation_path: /lovelace/ROOM_PATH
        variables:
          label_use_temperature: true
          label_use_brightness: false
          humidity: sensor.HUMIDITY_ROOM_NAME
          entity_1: # THIS EXAMPLE IS FOR CONTROLLING CLIMATE IN THAT ROOM
            entity_id: climate.THERMOSTAT_ROOM_NAME
            templates:
              - climate # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: more-info
            hold_action:
              action: none
            double_tap_action:
              action: none
            icon: mdi:ICON_NAME
          entity_2: # THIS EXAMPLE IS FOR TURNING OFF ALL LIGHTS IN THAT ROOM
            entity_id: sensor.LIGHTS_ROOM_NAME # SEE 1.2.2 IN THIS GUIDE TO CREATE THIS HELPER
            templates:
              - light
            tap_action:
              action: call-service
              service: light.toggle
              service_data:
                area_id:
                  - ROOM_NAME
            hold_action:
              action: none
            double_tap_action:
              action: none
          entity_3: # THIS EXAMPLE IS FOR A CONFIGURED APPLE TV IN HA
            entity_id: media_player.REPLACETHIS
            templates:
              - media_player # THIS VALUE CHANGE THE COLOR OF YOUR BUTTON DEPENDING ON STATE, MUST BE SETUP IN ROOM-CARD.YAML
            tap_action:
              action: navigate
              navigation_path: /lovelace/ROOM_PATH
            hold_action:
              action: call-service
              service: remote.send_command
              service_data:
                device_id: REPLACETHIS
                command:
                  - suspend
                  - wakeup
              icon: mdi:television # EXAMPLE FOR MEDIA CENTER
            double_tap_action:
              action: none
```
