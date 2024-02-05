# Room

Rooms are meant to be only accessible from the room cards on the home page. At first I found it unpractical to search through the navigation menu your room. This way you can use the navigation menu as a quick access bar with icons.

For each sections, excluding the header, I'm using a [Mushroom Title Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/title.md) setup like this:

```
type: custom:mushroom-title-card
title: ''
subtitle: SECTION_NAME
```

![image](https://github.com/gravelfreeman/graphite-dashboard-mobile/assets/44218371/6d88d072-b9ad-4b69-81ee-3e3d49960880)

## 1. Header

Follow these [instructions](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/header.md) to setup the header.

## 2. Lightning

To keep the UI clean I'm regrouping all the lights into a single block using the [stack-in-card](https://github.com/custom-cards/stack-in-card).

For the lights themselves I'm using [Mushroom Light Cards](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/light.md).

I also like to customize the icon depending if it's a switch, a light bulb or a special light.

```
type: custom:stack-in-card
mode: vertical
cards:
  - type: custom:mushroom-light-card
    entity: light.REPLACETHIS
    name: REPLACETHIS
    layout: horizontal
    fill_container: false
    primary_info: name
    show_brightness_control: true
    show_color_control: false
    show_color_temp_control: false
    use_light_color: false
    collapsible_controls: false
    icon: mdi:light-switch
    icon_color: amber
  - type: custom:mushroom-light-card
    entity: light.REPLACETHIS
    name: REPLACETHIS
    layout: horizontal
    fill_container: false
    primary_info: name
    show_brightness_control: true
    show_color_control: false
    show_color_temp_control: false
    use_light_color: false
    collapsible_controls: false
    icon: mdi:light-switch
    icon_color: amber
```

## 3. Devices and appliances



## 4. Climate

For controlling the climate/thermostats, I'm using a [Mushroom Climate Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/climate.md).

```
type: custom:mushroom-climate-card
entity: climate.THERMOSTAT_ID
icon: mdi:radiator
layout: horizontal
fill_container: false
primary_info: state
collapsible_controls: false
hvac_modes:
  - heat
show_temperature_control: true
secondary_info: name
name: Thermostat
```

## 5. Scheduler

This is one of my favorite component which gives a shortcut for non tech people in your household to create simple and advanced automations. You need to install the [scheduler-component](https://github.com/nielsfaber/scheduler-component) alongside of the [scheduler-card](https://github.com/nielsfaber/scheduler-card).

In this example you can see that I've restricted the scheduler component to the lights and thermostat of the current room. This is a neat way of making the rooms unique and only containing the information relevant.

I suggest you paste my code and go back to the visual editor since it's much easier to add your entities this way.

```
type: vertical-stack
cards:
  - type: custom:scheduler-card
    include:
      - climate.THERMOSTAT_NAME
      - light.EXAMPLE_ID
      - light.EXAMPLE_ID
      - light.EXAMPLE_ID
    title: true
    discover_existing: false
    sort_by:
      - state
      - relative-time
    display_options:
      primary_info: default
      secondary_info:
        - relative-time
      icon: action
    show_header_toggle: false
```

![image](https://github.com/gravelfreeman/graphite-dashboard-mobile/assets/44218371/e8bffcc4-7773-4f8f-9c79-37ae35a0565b)
