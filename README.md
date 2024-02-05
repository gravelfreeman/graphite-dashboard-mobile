# ha-mobile-dashboard
Sharing my custom dashboard since my friend asked for it. Since he offered me to help with the code I thought it would make sense to host it on my Github. Might as well share it publicly so that more people can help.

The goal of this mobile dashboard is to fix 2 things that I think are missing from Home Assistant. First looking good. Second being actually practical. Home-Assistant is a very good tool but I think it's ugly and all over the place. This dashboard isn't ready all of the box. You'll still have the pleasure to setup and learn Home-Assistant to your liking. It's more like a way to organize your dashbaord to achieve a clean look while keeping it practical.

## 1. Header

On all the pages I'm using Mushroom Chips Card for showing various information. On the homepage I like to show the outdoor temperature, alarm system and opened lights. For the rooms I'm showing the room temperature, room humidity and opened lights.

## 2. Main Page

The main page is dynamic which means that I'm only showing the widgets that are in an active state from the other rooms.

I like to show a greeting using the Mushroom Title Card even though it's completely useless. It kinda help formatting the page and make it look like an official device. In the subtitle of that card I'm declaring what's up for dinner tonight. You can add this code in the subtitle to do the same if you have Mealie and followed their HA instructions.

``Le souper de ce soir sera {{ states('sensor.mealie_todays_meal') }}.``

Right under I like to show who's home or away with Mushroom Person Cards. You can achieve it with an horizontal stack and this code example

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

### 2.1 Dynamic Cards

A few examples would be if there's music playing, if there's laundry ongoing, etc. You can quickly set this up by copy pasting cards from your room and integrate them in a conditional card. Set the condition so that it only appears if the state is active.

### 2.2 Rooms

Here I'm using UI-Lovelace-Minimalist room cards but without UI-Lovelace-Minimalist which I find it to be too much overhead and I like to keep my install as minimal as possible. You can find code here which you must add to your dashboard raw yaml configuration file. Those room cards are the only way to navigate the rooms in the dashboard which leave the dashboard menu to be used for other stuff. The card will show:

- Up to 4 icons of your choice
- Temperature / Humidity of the room
- Name and room icon

I like to use the first icon for the climate control and the second icon for lights control. Basically those icons are a shortcut to avoid navigating in the room and control that room faster.

