# Configuration notes 

## Mopidy 

### Addon

Install Mopidy addon using https://github.com/bestlibre/hassio-addons repo. This is an outdated repo, but still works. 
If you want to use Spotify, generate a token using: https://mopidy.com/authenticate/#spotify
These values should be entered below in the config. 

### Configuration 

-----


```yaml
local_scan: true
certfile: ca_bundle.crt
options:
  - name: local/media_dir
    value: /share/mopidy/media
  - name: m3u/playlists_dir
    value: /share/mopidy/playlists
  - name: spotify/username
    value: <username>
  - name: spotify/password
    value: <password>
  - name: spotify/client_id
    value: <client id>
  - name: spotify/client_secret
    value: <client secret>
```

This configuration allows local mp3 files to be discovered in `/share/mopidy/media`. 
You can add files here using a samba file share for example. 

The mopidy addon comes with two web ui interfaces: 
http://homeassistant.local:6680/iris
http://homeassistant.local:6680/moped

### Add Media player

To play files on an event we need to add a player to Home Assistant. This can be configured using the [Music Player Daemon](https://www.home-assistant.io/integrations/mpd/).

Simply add the following configuration to `configuration.yaml`
```yaml
media_player:
   - platform: mpd
     host: localhost
```

This will add a mpd player entity to Home Assistant (after restart). 

You can test if the player works correctly by triggering the service using the developer tools: 

```yaml
entity_id: media_player.mpd
media_content_id: local:track:sirene1.mp3
media_content_type: music
```

The file `sirene1.mp3` is located in `/share/mopidy/media/sirene1.mp3`.

To play a song from spotify it get's a bit more complicated. You need the special spotify id to play this song. Best way to figure this out is to go to the [iris web ui](http://homeassistant.local:6680/iris). After each song there's an ... option and if you click on it you can copy the URI. This URI is the `media_content_id` you need above.
For example `Metallica - The Four Horsemen` has the URI : `spotify:track:5RrnDgjoFxDINI5aFQPZXt` on my account. 
I can call this using: 
```yaml
entity_id: media_player.mpd
media_content_id: spotify:track:5RrnDgjoFxDINI5aFQPZXt
media_content_type: music
```

