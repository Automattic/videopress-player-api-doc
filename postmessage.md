## Postmessage Events

The VideoPress player has a series of `postMessage` events that will fire, based on various state changes that occur in the life cycle of a video. It also listens for some specific `postMessage` events to allow it to be controlled by the page it is embedded in.

The target where the events are emitted is the global window object, and also its parent if the video is embedded in an iframe.

```js
window.addEventListener( 'message', function( event ) {
  const eventData = event?.data;
  const { event: eventName } = eventData;
  console.log( `%s emitted`, eventName );
} );
```

### These are the events that fire:

**videopress_loading_state**

This event is fired when the video is being uploaded.

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

- converting
    - Type: `boolean`

- fetchingSubs
    - Type: `boolean`

- processing
    - Type: `boolean`

- state
    - Type: `boolean`
    - Values: `loaded`

**videopress_toggle-source**

This event is fired when the video quality changes.

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

- qualityLevelInternalName
    - Type: `string`
    - Values: `std`, `dvd`, `hd`, `hd_1080`

**videopress_playing**

The playing event is fired after playback is first started, and whenever it is restarted. For example it is fired when playback resumes after having been paused or delayed due to lack of data.

[`playing` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/playing_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

**videopress_pause**

The pause event is sent when a request to pause an activity is handled and the activity has entered its paused state.

[`pause` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/pause_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

**videopress_ended**

The ended event is fired when playback or streaming has stopped because the end of the media was reached or because no further data is available.

[`ended` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/ended_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

**videopress_timeupdate**

The timeupdate event is fired when the time indicated by the currentTime attribute has been updated.

[`timeupdate` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/timeupdate_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

- currentTime
    - Current time of the video, in seconds.

- currentTimeMs
    - Current time of the video, in milliseconds.

**videopress_seeking**

The seeking event is fired when a seek operation starts, meaning the Boolean seeking attribute has changed to true and the media is seeking a new position.

[`seeking` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/seeking_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

**videopress_volumechange**

The volumechange event is fired when the volume has changed.

[`volumechange` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/volumechange_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

**videopress_durationchange**

The `durationchange` event is fired when the duration attribute of the video has been updated.

[`durationchange` native event - MDN doc](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/durationchange_event)

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

- duration
    - Duration of the video, in seconds.

- durationMs
    - Duration of the video, in milliseconds.

**toggle_fullscreen**

The `toggle_fullscreen` event is fired immediately after the video player switches into or out of fullscreen mode.

`event.data` props:
- event
    - Type: `string`
    - The name of the event.

- id
    - Type: `string`
    - Globally unique identifier of the VideoPress video resource.

- isFullScreen
    - Type: `boolean`
    - Whether the video is in full-screen mode, or not.

### Events The Player Accepts

**videopress_action_play**

Starts video playback.

**videopress_action_pause**

Pauses the video and fires off a response `postMessage` event with the time the video was paused at.

`event.data` props for the response `postMessage` event
- event
    - Type: `string`
    - Value: `action_pause_response`
- currentTime
    - Type: `int`
    - the current time in seconds
- currentTimeMs
    - Type: `int`
    - the current time in milliseconds

**videopress_action_set_currenttime**

Sets the playhead to the specified time, if it is within the range of the video (between 0 and video's duration)

Accepts the following `event.data` props:
- currentTime
    - Type: `float`
    - The time in seconds that the playhead should be moved to

