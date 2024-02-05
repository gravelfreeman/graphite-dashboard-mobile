# Graphite Mobile Dashboard for Home-Assistant
Sharing my custom dashboard since my friend asked for it. Since he offered me to help with the code I thought it would make sense to host it on my Github. Might as well share it publicly so that more people can help.

The goal of this mobile dashboard is to fix 2 things that I think are missing from Home Assistant. First looking good. Second being actually practical. Home-Assistant is a very good tool but I think it's ugly and all over the place. This dashboard isn't ready all of the box. You'll still have the pleasure to setup and learn Home-Assistant to your liking. It's more like a way to organize your dashboard to achieve a clean look while keeping it practical.

# Demo

Coming soon...

# Features

- Homepage will be the starting point to quickly access everything
  - Swipe the screen with your finger to navigate the top menu
  - You're welcomed and told what's up for dinner tonight
  - Quickly shows who's home or away
  - Dynamic widgets that only appears when active
  - Every rooms with quick access icons to control climate, lights and devices without leaving the homepage
- Each page have an header that serves as a quick access to important stuff
- Navigation menu is used for a clean and quick access to general home automation
  - Security system integrates Alarmo, Frigate and shows all your sensors
  - Media center shows full artwork, media controls and list your playlists that can be triggered
  - Lights will show all your lights and let you control them
  - Energy will send you to the energy dashboard without using the hamburger menu
- Clicking on a room gives more control to that room
  - List and control all the devices in that room
  - List and control all the lights in that room
  - Control your climate/thermostat in that room
  - Setup climate and lightning automations from the dashboard

# Table of Content

- [1. Home](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md)
  - [1.1 Header](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md)
  - [1.2 Greetings](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md#2-greetings)
  - [1.3 People](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md#3-people)
  - [1.4 Dynamic Cards](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md#4-dynamic-cards)
  - [1.5 Rooms](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md#5-rooms)
    - [1.5.1 Room Card Template](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md#51-add-the-card-room-template)
    - [1.5.2 Add Room Card to Dashboard](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/home.md#52-add-a-custom-card-room-in-your-dashboard)
- [2. Room](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/room.md)
  - [2.1 Header](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/room.md#1-header)
  - [2.2 Lightning](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/room.md#2-lightning)
  - [2.3 Devices and appliances](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/room.md#3-devices-and-appliances)
  - [2.4 Climate](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/room.md#4-climate)
  - [2.5 Scheduler](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/room.md#4-climate)
- [3. Security](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/security.md)
  - [3.1 Header](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/security.md#1-header)
  - [3.2 Security System](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/security.md#2-security-system)
  - [3.3 Camera NVR](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/security.md#3-camera-nvr)
  - [3.4 Sensors](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/security.md#4-sensors)
- [4. Media](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/media.md)
  - [4.1 Media controls and Artwork](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/media.md#1-media-controls-and-artwork)
  - [4.2 Playlist Selection](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/media.md#2-playlist-selection)
- [5. Lights](https://github.com/gravelfreeman/graphite-dashboard-mobile/blob/main/docs/lights.md)
- [6. Energy]()

# Requirements

#### Theme

- [graphite-theme](https://github.com/gravelfreeman/graphite-theme) - Works best using my custom theme

#### Required frontend components

- [hass-swipe-navigation](https://github.com/zanna-37/hass-swipe-navigation) - Gives the ability to swipe using the touch screen
- [lovelace-card-mod](https://github.com/thomasloven/lovelace-card-mod) - Used to style the UI
- [lovelace-mushroom](https://github.com/piitaya/lovelace-mushroom) - This project is based on Mushroom cards
- [stack-in-card](https://github.com/custom-cards/stack-in-card) - Used to group elements in the UI like the lights

#### Optional integrations

- [alarmo](https://github.com/nielsfaber/alarmo) - Used for the security system
- [frigate-hass-integration](https://github.com/blakeblackshear/frigate-hass-integration) - Used for the camera/NVR system
- [scheduler-component](https://github.com/nielsfaber/scheduler-component) - Used for creating automations within a room page
- [spotify](https://www.home-assistant.io/integrations/spotify/) - Essential if using Spotcast
- [spotcast](https://github.com/fondberg/spotcast) - Connect your Spotify account

#### Optional frontend components

- [scheduler-card](https://github.com/nielsfaber/scheduler-card) - Used in conjunction with scheduler-component
- [frigate-hass-card](https://github.com/dermotduffy/frigate-hass-card/releases) - Used in conjunction with Frigate
- [spotify-card](https://github.com/custom-cards/spotify-card) - Used in conjunction with Spotcast
- [mini-media-player](https://github.com/kalkih/mini-media-player) - Used to display the artwork and media controls

# Extras
