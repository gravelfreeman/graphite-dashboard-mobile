# Media

I'll be honest this section is pretty much broken and I hadn't much time to make it work. My media center is an Apple TV and right now I'm stuck in getting pyATV to open Spotify playlist links from a Home-Assitant button to the Apple TV. This could replace spotify-card which can't start a playlist on your device in a single click. For example in my case I need a button in order to turn on the amp on the right input, open the television, open the Apple TV and then send the playlist command.

I opened [this issue](https://github.com/postlund/pyatv/issues/2311) on pyATV, please raise your voice if you want me to build a button template to automate starting playlists on an Apple TV!

![image](https://github.com/gravelfreeman/graphite-dashboard-mobile/assets/44218371/e508b569-c5d1-4df7-8354-c64ca466bc03)

## 1. Media controls and Artwork

The top section is showing the Artwork of what's currently playing on the media center. I'm using [Mini Media Player](https://github.com/kalkih/mini-media-player) to achieve this. I've removed everything I could and kept it minimal. If someone knows how to remove the right arrow let me know since it's not useful.

```
artwork: full-cover
entity: media_player.YOUR_MEDIA_PLAYER_ID
hide:
  power: true
  progress: false
  runtime: true
  volume: true
  name: true
  icon: true
  info: false
  controls: false
type: custom:mini-media-player
toggle_power: false
volume_stateless: false
group: false
source: full
info: scroll
replace_mute: stop
sound_mode: icon
tap_action:
  - action: none
```

## 2. Playlist Selection

This section is mean't to be replaced by a better option to automate opening your media center, television and amplifier. Right now if you still want this the way it's setup on the screenshot I'm using [spotify-card](https://github.com/custom-cards/spotify-card). At first it was looking like a complete mess until I went on Spotify and setup a clean image for each of my playlists. I suggest you do the same if, again, you want the UI to look clean and professional.

```
type: custom:spotify-card
limit: 50
default_device: media_player.YOUR_MEDIA_PLAYER_ID
style: |
  #content{
      border:none !important;
      background-color: transparent !important;
  }
  .list-item{
      border-radius: 0.2vw;
      overflow:hidden;
      border:none !important;
      margin-bottom:3px;
  }
  .list-item.playing {
      background-color: var(--secondary-background-color) !important;
  }
hide_chromecast_devices: true
include_playlists:
  - PLAYLIST_NAME # ENTER ALL YOUR PLAYLISTS NAME HERE
  - PLAYLIST_NAME
  - PLAYLIST_NAME
hide_warning: true
hide_currently_playing: true
hide_playback_controls: true
always_play_random_song: true
```
