# Lights

I like to have a page dedicated to lights in my home and it's this page that you're redirected when clicking on the light icon of the home header. This makes the whole thing coesive and holding up together.

![image](https://github.com/gravelfreeman/graphite-dashboard-mobile/assets/44218371/4bcdb917-2f9c-4b03-b484-64fbe8d791bb)

Using an Horizontal Stack you can add 3 lights per row. Here's a template for it using [Mushroom Light Card](https://github.com/piitaya/lovelace-mushroom/blob/main/docs/cards/light.md).

```
type: horizontal-stack
title: ROOM_NAME
cards:
  - type: custom:mushroom-light-card
    entity: light.NAME
    layout: vertical
    use_light_color: true
    show_brightness_control: false
    show_color_temp_control: false
    show_color_control: false
    collapsible_controls: false
    icon_color: amber
    name: REPLACETHIS
  - type: custom:mushroom-light-card
    entity: light.NAME
    layout: vertical
    use_light_color: true
    show_brightness_control: false
    show_color_temp_control: false
    show_color_control: false
    collapsible_controls: false
    icon_color: amber
    name: REPLACETHIS
  - type: custom:mushroom-light-card
    entity: light.NAME
    layout: vertical
    use_light_color: true
    show_brightness_control: false
    show_color_temp_control: false
    show_color_control: false
    collapsible_controls: false
    icon_color: amber
    name: REPLACETHIS
```
